## Processors
Using std::thread::hardware_concurrency() directly requires care; your code doesn’t take into account any of the other threads that are running on the system unless you explicitly share that information. 

In the worst-case scenario, if multiple threads call a function that uses std::thread::hardware_concurrency() for scaling at the same time, there will be huge oversubscription.

 std::async() avoids this problem because the library is aware of all calls and can schedule appropriately. Careful use of thread pools can also avoid this problem

 But even if you take into account all threads running in your application, you’re still subject to the impact of other applications running at the same time.
 
  Although the use of multiple CPU-intensive applications simultaneously is rare on single-user systems, there are some domains where it’s more common. Systems designed to handle this scenario typically offer mechanisms to allow each application to choose an appropriate number of threads, although these mechanisms are outside the scope of the C++ Standard. One option is for a facility like std::async() to take into account the total number of asynchronous tasks run by all applications when choosing the number of threads. 
 
 Another is to limit the number of processing cores that can be used by a given application. I’d expect this limit to be reflected in the value returned by std::thread::hardware_concurrency() on these platforms, although this isn’t guaranteed. If you need to handle this scenario, consult your system documentation to see what options are available to you

 ## Data contention and cache ping-pong

 If two threads are executing concurrently on different processors and they’re both reading the same data, this usually won’t cause a problem; the data will be copied into their respective caches, and both processors can proceed. But if one of the threads modifies the data, this change then has to propagate to the cache on the other core, which takes time. Depending on the nature of the operations on the two threads, and the memory orderings used for the operations, this modification may cause the second processor to stop in its tracks and wait for the change to propagate through the memory hardware. In terms of CPU instructions, this can be a phenomenally slow operation, equivalent to many hundreds of individual instructions, although the exact timing depends primarily on the physical structure of the hardware.

Consider the following simple piece of code:

```
std::atomic<unsigned long> counter(0);
void processing_loop()
{
    while(counter.fetch_add(1,std::memory_order_relaxed)<100000000)
    {
        do_something();
    }
}
```

The counter is global, so any threads that call processing_loop() are modifying the same variable. Therefore, for each increment the processor must ensure it has an up-to-date copy of counter in its cache, modify the value, and publish it to other processors. Even though you’re using std::memory_order_relaxed, so the compiler doesn’t have to synchronize any other data, fetch_add is a read-modify-write operation and therefore needs to retrieve the most recent value of the variable. If another thread on another processor is running the same code, the data for counter must therefore be passed back and forth between the two processors and their corresponding caches so that each processor has the latest value for counter when it does the increment. If do_something() is short enough, or if there are too many processors running this code, the processors might find themselves waiting for each other; one processor is ready to update the value, but another processor is currently doing that, so it has to wait until the second processor has completed its update and the change has propagated. This situation is called high contention. If the processors rarely have to wait for each other, you have low contention.

In a loop like this one, the data for counter will be passed back and forth between the caches many times. This is called cache ping-pong, and it can seriously impact the performance of the application. If a processor stalls because it has to wait for a cache transfer, it can’t do any work in the meantime, even if there are other threads waiting that could do useful work, so this is bad news for the whole application

## False sharing
Suppose you have an array of int values and a set of threads that each access their own entry in the array but do so repeatedly, including updates. Because an int is typically much smaller than a cache line, quite a few of those array entries will be in the same cache line. Consequently, even though each thread only accesses its own array entry, the cache hardware still has to play cache ping-pong

Every time the thread accessing entry 0 needs to update the value, ownership of the cache line needs to be transferred to the processor running that thread, only to be transferred to the cache for the processor running the thread for entry 1 when that thread needs to update its data item. The cache line is shared, even though none of the data is, hence the term false sharing

The C++17 standard defines 

std::hardware_destructive_interference_size in the header <new>, which specifies the maximum number of consecutive bytes that may be subject to false sharing for the current compilation target. If you ensure that your data is at least this number of bytes apart, then there will be no false sharing

## Oversubscription and execessive task switching
