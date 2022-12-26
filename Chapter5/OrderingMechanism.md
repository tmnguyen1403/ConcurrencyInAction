## Summary

```
std::thread
The completion of the std::thread constructor synchronizes with the invocation of the supplied function or callable object on the new thread.
The completion of a thread synchronizes with the return from a successful call to join on the std::thread object that owns that thread.

std::mutex, std::timed_mutex, std::recursive_mutex, std::recursive_timed_mutex
All calls to lock and unlock, and successful calls to try_lock, try_lock_for, or try_lock_until, on a given mutex object form a single total order: the lock order of the mutex.
A call to unlock on a given mutex object synchronizes with a subsequent call to lock, or a subsequent successful call to try_lock, try_lock_for, or try_lock_until, on that object in the lock order of the mutex.
Failed calls to try_lock, try_lock_for, or try_lock_until do not participate in any synchronization relationships.


std::shared_mutex, std::shared_timed_mutex
All calls to lock, unlock, lock_shared, and unlock_shared, and successful calls to try_lock, try_lock_for, try_lock_until, try_lock_shared, try_lock_shared_for, or try_lock_shared_until, on a given mutex object form a single total order: the lock order of the mutex.

A call to unlock on a given mutex object synchronizes with a subsequent call to lock or shared_lock, or a successful call to try_lock, try_lock_for, try_lock_until, try_lock_shared, try_lock_shared_for, or try_lock_shared_until, on that object in the lock order of the mutex.

Failed calls to try_lock, try_lock_for, try_lock_until, try_lock_shared, try_lock_shared_for, or try_lock_shared_until do not participate in any synchronization relationships.

std::promise, std::future and std::shared_future

The successful completion of a call to set_value or set_exception on a given std::promise object synchronizes with a successful return from a call to wait or get, or a call to wait_for or wait_until that returns std::future_status:: ready on a future that shares the same asynchronous state as the promise.

The destructor of a given std::promise object that stores an std::future_error exception in the shared asynchronous state associated with the promise synchronizes with a successful return from a call to wait or get, or a call to wait_for or wait_until that returns std::future_status::ready on a future that shares the same asynchronous state as the promise.


std::packaged_task, std::future and std::shared_future

The successful completion of a call to the function call operator of a given std::packaged_task object synchronizes with a successful return from a call to wait or get, or a call to wait_for or wait_until that returns std::future_status::ready on a future that shares the same asynchronous state as the packaged task.

The destructor of a given std::packaged_task object that stores an std:: future_error exception in the shared asynchronous state associated with the packaged task synchronizes with a successful return from a call to wait or get, or a call to wait_for or wait_until that returns
std::future_status::ready on a future that shares the same asynchronous state as the packaged task.


std::async, std::future and std::shared_future

The completion of the thread running a task launched via a call to std::async with a policy of std::launch::async synchronizes with a successful return from a call to wait or get, or a call to wait_for or wait_until that returns std::future_status::ready on a future that shares the same asynchronous state as the spawned task.

The completion of a task launched via a call to std::async with a policy of std::launch::deferred synchronizes with a successful return from a call to wait or get, or a call to wait_for or wait_until that returns std::future_status::ready on a future that shares the same asynchronous state as the promise.


std::experimental::future, std::experimental::shared_future and continuations
The event that causes an asynchronous shared state to become ready synchronizes with the invocation of a continuation function scheduled on that shared state.

The completion of a continuation function synchronizes with a successful return from a call to wait or get, or a call to wait_for or wait_until that returns std::future_status::ready on a future that shares the same asynchronous state as the future returned from the call to then that scheduled the continuation, or the invocation of any continuation scheduled on that future.

std::experimental::latch

The invocation of each call to count_down or count_down_and_wait on a given instance of std::experimental::latch synchronizes with the completion of each successful call to wait or count_down_and_wait on that latch.

std::experimental::barrier

The invocation of each call to arrive_and_wait or arrive_and_drop on a given instance of std::experimental::barrier synchronizes with the completion of each subsequent successful call to arrive_and_wait on that barrier.

std::experimental::flex_barrier

The invocation of each call to arrive_and_wait or arrive_and_drop on a given instance of std::experimental::flex_barrier synchronizes with the completion of each subsequent successful call to arrive_and_wait on that barrier.
The invocation of each call to arrive_and_wait or arrive_and_drop on a given instance of std::experimental::flex_barrier synchronizes with the subsequent invocation of the completion function on that barrier.

The return from the completion function on a given instance of std:: experimental::flex_barrier synchronizes with the completion of each call to arrive_and_wait on that barrier that was blocked waiting for that barrier when the completion function was invoked.


std::condition_variable and std::condition_variable_any
Condition variables do not provide any synchronization relationships. They are optimizations over busy-wait loops, and all the synchronization is provided by the operations on the associated mutex.
```