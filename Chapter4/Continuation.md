## Then feature in C++
ChatGPT answer

In C++20, the std::future class template was extended with a new member function called std::future::then, which allows you to specify a continuation to be executed when the std::future object is ready.

A continuation is a function or callable object that is executed when the result of a std::future object becomes available. The continuation can be used to chain multiple asynchronous operations together, allowing you to write code that executes asynchronously in a more sequential and intuitive manner.

Here is an example of how to use the std::future::then function to specify a continuation for a std::future object
```
#include <future>
#include <iostream>

int Multiply(int x, int y) {
  return x * y;
}

int main() {
  std::future<int> result = std::async(Multiply, 2, 3);
  auto continuation = result.then([](auto&& f) {
    int result = f.get();
    std::cout << "Result: " << result << std::endl;
    return result * 2;
  });

  std::cout << "Continuation: " << continuation.get() << std::endl;  // Outputs "Continuation: 12"

  return 0;
}

```

In this example, the Multiply function is launched asynchronously using std::async, and the returned std::future object is stored in the result variable.

The result.then function is used to specify a continuation that will be executed when the result of the result std::future object becomes available. The continuation is a lambda function that retrieves the result of the result std::future object using the get member function, prints it to the console, and returns the result multiplied by 2.