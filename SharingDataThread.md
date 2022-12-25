### Useful terms
Invariants: statements that are always true about a particular data structure

Example: Doubly linked list invariant:

If you follow a next pointer from one node (A) to another (B), the previous pointer from that node (B) points back to the first node (A).

Therefore, if we delete a node, the prior node of deleted node and the later node need to be updated to point to each other.

The invariant is broken while deleting a node. Therefore, if the list is shared between multiple threads, a reading thread and a modifying thread can cause several issues: skipped data, invalid memory access (for accessing the deleted node).

This problem is called race condition.

### Race conditions
_"Race conditions are generally timing-sensitive, they can often disappear entirely when the application is run under the debugger, because the debugger affects the timing of the program, even if only slightly"

### Avoiding problematic race conditions
_Modifications in the data structure are done as a series of indivisilbe changes, each of which preserves the invariants.
(lock-free programming)

_Handle data updates as a transaction. (Software transactional memory - STM)

### Mutex
_Mutex can be used to protect data. However, if a member function return a reference to a shared data member, the shared data member is now can be modified without any lock --> backdoor is created.

"Don’t pass pointers and references to protected data outside the scope of the lock, whether by returning them from a function, storing them in externally visible memory, or passing them as arguments to user-supplied functions."