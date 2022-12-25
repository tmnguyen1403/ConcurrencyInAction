## C++ Solution
```
std::shared_ptr<some_resource> resource_ptr;
std::once_flag resource_flag;                       1
void init_resource()
{
    resource_ptr.reset(new some_resource);
}
void foo()
{
    std::call_once(resource_flag,init_resource);    2
    resource_ptr->do_something();
}
```
1. std::call_one have a lower overhead than using a mutex explicitly
2. When std::call_once returns, the pointer will have been initialized by some thread. Then synchronization data is stored in std::once_flag instance.

## Local static variable initialization is thread-safe
```
class my_class;
my_class& get_my_class_instance()
{
    static my_class instance;        1
    return instance;
}
```
