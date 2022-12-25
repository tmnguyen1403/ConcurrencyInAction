## Locks
1. std::lock_guard
2. std::unique_lock
_ allow to defer lock acquire
```
std::unique_lock lock_a(mutex, std::defer_lock)
std::lock(lock_a);
```
The std::defer_lock options prevent the lock acquisition during initialization. We can later pass the lock into lock to start acquiring the mutex. However, since the flag information needs to be stored somewhere, std::unique_lock performance is worse than std::lock_guard.

Holding a lock longer than necessary might decrease performance since other threads are waiting for the lock. Therefore, it is sometimes necessary to call unlock manually.

