### Exception handling with
```
std::async and std::packaged_task
Exceptions are stored in the result

std::promise

try
{
    some_promise.set_value(calculate_value());
}
catch(...)
{
    some_promise.set_exception(std::current_exception());
    //OR
    some_promise.set_exception(std::make_exception_ptr(std::logic_error("foo ")));
}
```

If "std::promise or std::packaged_task associated with the future is destroyed without calling either of the set functions on the promise or invoking the packaged task, the exeception is stored - std::future_error - error code std::future_errc::broken_promise"

