## Use std::packaged_task and std::future
std::packaged_task throws the exeception to std::future.
Therefore, when performing std::future.get(), the main thread can retrieve the exception.

## std::async
Works the same like std::packaged_task
```
Listing 8.5. An exception-safe parallel version of std::accumulate using std::async
template<typename Iterator,typename T>
T parallel_accumulate(Iterator first,Iterator last,T init)
{
    unsigned long const length=std::distance(first,last);              1
    unsigned long const max_chunk_size=25;
    if(length<=max_chunk_size)
    {
        return std::accumulate(first,last,init);                      2
    }
    else
    {
    Iterator mid_point=first;
        std::advance(mid_point,length/2);                             3
        std::future<T> first_half_result=
            std::async(parallel_accumulate<Iterator,T>,               4
                       first,mid_point,init);
        T second_half_result=parallel_accumulate(mid_point,last,T()); 5
        return first_half_result.get()+second_half_result;            6
    }
}
```