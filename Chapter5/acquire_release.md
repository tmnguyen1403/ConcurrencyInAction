## Acquire-release ordering
```

Acquire-release ordering is a step up from relaxed ordering; there’s still no total order of operations, but it does introduce some synchronization.

Under this ordering model, 

atomic loads are acquire operations (memory_order_acquire), 

atomic stores are release operations (memory_order_release), and

atomic read-modify-write operations (such as fetch_add() or exchange()) are either acquire, release, or both (memory_order_acq_rel)

```

Listing 5.8. Acquire-release operations can impose ordering on relaxed operations
```
#include <atomic>
#include <thread>
#include <assert.h>
std::atomic<bool> x,y;
std::atomic<int> z;
void write_x_then_y()
{
    x.store(true,std::memory_order_relaxed);           1
    y.store(true,std::memory_order_release);           2
}
void read_y_then_x()
{
    while(!y.load(std::memory_order_acquire));         3
    if(x.load(std::memory_order_relaxed))              4
        ++z;
}
int main()
{
    x=false;
    y=false;
    z=0;
    std::thread a(write_x_then_y);
    std::thread b(read_y_then_x);
    a.join();
    b.join();
    assert(z.load()!=0);                              5
}
```
3 Spin, waiting for y to be set to true
Eventually, the load from y, 3 will see true as written by the store 2. Because the store uses memory_order_release and the load uses memory_order_acquire, the store synchronizes with the load. 

The store to x 1 happens before the store to y 2 because they’re in the same thread. Because the store to y synchronizes with the load from y, the store to x also happens before the load from y and by extension happens before the load from x 4. Thus, the load from x must read true, and the assert 5 can’t fire. 

If the load from y wasn’t in a while loop, this wouldn’t necessarily be the case; the load from y might read false, in which case there’d be no requirement on the value read from x. In order to provide any synchronization, acquire and release operations must be paired up. The value stored by a release operation must be seen by an acquire operation for either to have any effect. 

If either the store at 2 or the load at 3 was a relaxed operation, there’d be no ordering on the accesses to x, so there’d be no guarantee that the load at 4 would read true, and the assert could fire.