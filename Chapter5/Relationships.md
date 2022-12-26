## synchronizes-with relationship
The synchronizes-with relationship is something that you can get only between operations on atomic types. Operations on a data structure (such as locking a mutex) might provide this relationship if the data structure contains atomic types and the operations on that data structure perform the appropriate atomic operations internally, but fundamentally it comes only from operations on atomic types.

The basic idea is this: a suitably-tagged atomic write operation, W, on a variable, x, synchronizes with a suitably-tagged atomic read operation on x that reads the value stored by either that write, W, or a subsequent atomic write operation on x by the same thread that performed the initial write, W, or a sequence of atomic read-modify-write operations on x (such as fetch_add() or compare_exchange_weak()) by any thread, where the value read by the first thread in the sequence is the value written by W 

## happens-before relationship

The happens-before and strongly-happens-before relationships are the basic building blocks of operation ordering in a program; it specifies which operations see the effects of which other operations

If the operations occur in the same statement, in general there’s no happens-before relationship between them, because they’re unordered.

```
Listing 5.3. Order of evaluation of arguments to a function call is unspecified
#include <iostream>
void foo(int a,int b)
{
    std::cout<<a<<","<<b<<std::endl;
}
int get_num()
{
    static int i=0;
    return ++i;
}
int main()
{
    foo(get_num(),get_num());   1
}
```

