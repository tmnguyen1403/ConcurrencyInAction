## Reader-writer mutex

```
std::shared_mutex: offer a performance benefit
std::shared_timed_mutex: supports additional o
perations
```

Listing 3.13 Protecting a data structure with std::shared_mutex
```
#include <map>
#include <string>
#include <mutex>
#include <shared_mutex>
class dns_entry;
class dns_cache
{
    std::map<std::string,dns_entry> entries;
    mutable std::shared_mutex entry_mutex;
public:
    dns_entry find_entry(std::string const& domain) const
    {
        std::shared_lock<std::shared_mutex> lk(entry_mutex);         1
        std::map<std::string,dns_entry>::const_iterator const it=
            entries.find(domain);
        return (it==entries.end())?dns_entry():it->second;
    }
    void update_or_add_entry(std::string const& domain,
                             dns_entry const& dns_details)
    {
        std::lock_guard<std::shared_mutex> lk(entry_mutex);          2
        entries[domain]=dns_details;
    }
};
```
When find_entry is called, the shared_lock acquires the entry_mutex. This prevents other thread calling the update_or_add_entry function to acquire the mutex, but threads that call find_entry still be able to acquire the mutex and perform the search.

On the other hand, when update_or_add_entry is called and the mutex is acquired, no other threads can perform update or find until the mutex is released.

## Recursive locking
```
std::recursive_mutex: you need to call unlock equals to the number of time call lock
```
```
A common use of recursive mutexes is where a class is designed to be accessible from multiple threads concurrently, so it has a mutex protecting the member data. Each public member function locks the mutex, does the work, and then unlocks the mutex. But sometimes it’s desirable for one public member function to call another as part of its operation. In this case, the second member function will also try to lock the mutex, leading to undefined behavior. The quick-and-dirty solution is to change the mutex to a recursive mutex. This will allow the mutex lock in the second member function to succeed and the function to proceed.
```