## Sequentially consistent
This is the default ordering
```
memory_order_seq_cst
```
If all operations on instances of atomic types are sequentially consistent, the behavior of a multithreaded program is as if all these operations were performed in some particular sequence by a single thread

### ChatGPT

In C++11 and later, atomic operations can specify a memory order constraint to indicate the level of memory visibility and synchronization that is required for the operation. The memory_order_seq_cst constraint specifies that an atomic operation has the highest level of memory visibility and synchronization, ensuring that all threads can see the effects of the operation in a sequentially consistent order.

```
#include <atomic>

std::atomic<int> x(0);
std::atomic<int> y(0);

void thread_1() {
  x.store(1, std::memory_order_seq_cst);
  y.store(1, std::memory_order_seq_cst);
}

void thread_2() {
  while (y.load(std::memory_order_seq_cst) == 0) {}
  int v = x.load(std::memory_order_seq_cst);
  // Use v...
}
```
In this example, the memory_order_seq_cst constraint ensures that thread_2 will see the effects of thread_1

## Acquire-release ordering
```
memory_order_consume
memory_order_acquire
memory_order_release
memory_order_acq_rel
```
### Relaxed ordering
```
memory_order_relaxed
```
"Without any additional synchronization, the modification order of each variable is the only thing shared between threads that are using memory_order_relaxed"

"relaxed atomic operations difficult to deal with. They must be used in combination with atomic operations that feature stronger ordering semantics in order to be useful for inter-thread synchronization. I strongly recommend avoiding relaxed atomic operations unless they’re absolutely necessary, and even then using them only with extreme caution. Given the unintuitive results that can be achieved with only two threads and two variables in listing 5.5, it’s not hard to imagine the possible complexity when more threads and more variables are involved."

### ChatGPT
The memory_order_relaxed constraint specifies that an atomic operation has the lowest level of memory visibility and synchronization, allowing the operation to be optimized for maximum performance.