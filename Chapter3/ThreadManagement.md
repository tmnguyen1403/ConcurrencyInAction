## Chapter 2

### Choosing the number of threas at runtime
```using std::thread::hardware_concurrency()```
_returns an indication of the number of threads that can truly run concurrently for a given execution of a program

_might return 0 if the information is not available

### Thread identifiers
```std::thread::id```
```std::this_thread::get_id()```

Id can also be hash using
```std::hash<std::thread::id>```
