```
A latch is a synchronization object that becomes ready when its counter is decremented to zero. Its name comes from the fact that it latches the output—once it is ready, it stays ready until it is destroyed. 
A latch is thus a lightweight facility for waiting for a series of events to occur
```
```
a barrier is a reusable synchronization component used for internal synchronization between a set of threads.
 Whereas a latch doesn’t care which threads decrement the counter—the same thread can decrement the counter multiple times, or multiple threads can each decrement the counter once, or some combination of the two—with barriers, each thread can only arrive at the barrier once per cycle. 
```

### Listing 4.25. Waiting for events with std::latch
```
void foo(){
    unsigned const thread_count=...;
    latch done(thread_count);                                    1
    my_data data[thread_count];
    std::vector<std::future<void> > threads;
    for(unsigned i=0;i<thread_count;++i)
        threads.push_back(std::async(std::launch::async,[&,i]{   2
            data[i]=make_data(i);
            done.count_down();                                   3
            do_more_stuff();                                     4
        }));
    done.wait();                                                 5
    process_data(data,thread_count);                             6
}                   
```

### Check Listing 4.26 and 4.27 for std::barrier and std::flex_barrier