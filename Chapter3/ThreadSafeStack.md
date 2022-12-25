### Handle pop

1. Pass in a reference
2. Require a no-throw copy constructor or move constructor
3. Return a pointer to the popped item

"std::shared_ptr"

_Cons: "returning a pointer requires a means of managing the memory allocated to the object, and for simple types such as integers, the overhead of such memory management can exceed the cost of returning the type by value"
4. Provide both option 1 and either option 2 or 3