## Check for lock_free
Since C++17, all atomic types have a static constexpr member variable, X::is_always_lock_free, which is true if and only if the atomic type X is lock-free for all supported hardware that the output of the current compilation might run on.

```
std::atomic_flag is always required to be lock-free.
test_and_set()
clear()
no assignment, no copy construction, no test and clear, no other operations
```
### Memory Order
```
Each of the operations on the atomic types has an optional memory-ordering argument which is one of the values of the std::memory_order enumeration. This argument is used to specify the required memory-ordering semantics. 
The std::memory_order enumeration has six possible values: std::memory_order_relaxed, std:: memory_order_acquire, std::memory_order_consume, std::memory_order_acq_rel, std::memory_order_release, and std::memory_order_seq_cst
```

