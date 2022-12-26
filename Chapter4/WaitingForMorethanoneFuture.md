## Using std::when_all
ChatGPT reply

std::when_all is a function template provided by the C++ Standard Library that allows you to wait for multiple asynchronous operations to complete. It takes a range of std::future or std::shared_future objects as input, and it returns a std::future object that becomes ready when all of the input std::future objects are ready.

Here is an example of how to use std::when_all to wait for multiple asynchronous operations to complete:
```
#include <future>
#include <iostream>
#include <vector>

int Multiply(int x, int y) {
  return x * y;
}

int main() {
  std::vector<std::future<int>> futures;
  futures.emplace_back(std::async(Multiply, 2, 3));
  futures.emplace_back(std::async(Multiply, 4, 5));

  std::future<void> when_all_result = std::when_all(futures.begin(), futures.end());
  when_all_result.wait();

  for (auto& f : futures) {
    std::cout << f.get() << std::endl;
  }

  return 0;
}

```
In this example, the Multiply function is launched asynchronously twice using std::async, and the returned std::future objects are stored in the futures vector.

The std::when_all function is then used to create a new std::future object that becomes ready when all of the std::future objects in the futures vector are ready. The when_all_result std::future object is passed to the wait member function to block the main thread until all of the asynchronous operations have completed.

Finally, the result of each asynchronous operation is retrieved using the get member function of the std::future objects, and the results are printed to the console.

I hope this helps clarify the use of std::when_all in C++. If you have any further questions, please don't hesitate to ask.
