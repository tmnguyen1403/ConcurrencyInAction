## Future concept
_Once an event has happened (and the future has become ready), the future can't be reset.

```
std::future<>: an instance of std::future can only refer to its associated event
```
```
std::shared_future<>: multiple instances of std::shared_future may refer to the same event
```

```
std::packaged_task vs std::async
```
### ChatGPT Reponse
```
std::packaged_task
Pros:

Allows you to execute a task synchronously or asynchronously, depending on how you invoke it.
Provides more control over the execution of the task, as it allows you to specify the execution policy (e.g., std::launch::async or std::launch::deferred) and to retrieve the associated std::thread object.
Allows you to store the result in a std::promise object, which can be used to set the result of the task from a different thread or context.
Provides a lower-level interface for creating and executing asynchronous tasks, which may be more efficient in certain cases.
Cons:

Requires more code and is more complex to use than std::async.
Does not provide automatic management of threads and resources, which can lead to more overhead and potential resource leaks if not used properly.
Does not provide exception handling by default, which means you have to handle exceptions manually if the task throws an exception.
std::async
Pros:
Executes a task asynchronously and automatically manages the underlying threads and resources needed to execute the task.
Provides a higher-level interface for creating and executing asynchronous tasks, which is easier to use and requires less code.
Provides exception handling by default, which means that any exceptions thrown by the task are propagated to the calling
```