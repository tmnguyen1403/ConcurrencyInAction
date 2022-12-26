### std::shared_future
_allows multiple threads can wait for the same event  
_copyable  
_not thread_safe  
_"Need to pass a copy of the shared_future object to each thread so each thread can access its own local shared_future object safely, as the internals are now correctly synchronized by the library."

### Example
Instances of std::shared_future that reference some asynchronous state are constructed from instances of std::future that reference that state. Since std::future objects don’t share ownership of the asynchronous state with any other object, the ownership must be transferred into the std::shared_future using std::move, leaving std::future in an empty state, as if it were a default constructor:
```
std::promise<int> p;
std::future<int> f(p.get_future());
assert(f.valid());                            1
std::shared_future<int> sf(std::move(f));
assert(!f.valid());                           2
assert(sf.valid()); 
```

1 The future f is valid.  
2 f is no longer valid.  
3 sf is now valid.  

std::future has a share() member function that creates a new std::shared_future and transfers ownership to it directly. This can save a lot of typing and makes code easier to change:

```
std::promise< std::map< SomeIndexType, SomeDataType, SomeComparator,
    SomeAllocator>::iterator> p;
auto sf=p.get_future().share();
```