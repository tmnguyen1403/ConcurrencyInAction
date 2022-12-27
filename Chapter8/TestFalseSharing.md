One way to test whether this kind of false sharing is a problem is to add huge blocks of padding between the data elements that can be concurrently accessed by different threads. For example, you can use
```
struct protected_data
{
    std::mutex m;
    char padding[std::hardware_destructive_interference_size];     1
    my_data data_to_protect;
};
```
1 If std::hardware_destructive_interference_size is not available with your compiler, you could use something like 65536 bytes which is likely to be orders of magnitude larger than a cache line
to test the mutex contention issue or
```
struct my_data
{
    data_item1 d1;
    data_item2 d2;
    char padding[std::hardware_destructive_interference_size];
};
my_data some_array[256];
```
to test for false sharing of array data. If this improves the performance, you know that false sharing was a problem, and you can either leave the padding in or work to eliminate the false sharing in another way by rearranging the data accesses.