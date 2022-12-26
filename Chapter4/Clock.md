## Clock class
 a clock is a class that provides four distinct pieces of information:
```
The time now
The type of the value used to represent the times obtained from the clock
The tick period of the clock
Whether or not the clock ticks at a uniform rate and is therefore considered to be a steady clock
```

```
a clock that ticks 25 times per second has a period of std::ratio<1,25>, 
whereas a clock that ticks every 2.5 seconds has a period of std::ratio<5,2>

Note:
There’s no guarantee that the observed tick period in a given run of the program matches the specified period for that clock.
```

### Duration
```
The first template parameter is the type of the representation (such as int, long, or double), and the second is a fraction specifying how many seconds each unit of the duration represents.
 For example, a number of minutes stored in a short is std::chrono::duration<short,std:: ratio<60,1>>, because there are 60 seconds in a minute.
  On the other hand, a count of milliseconds stored in a double is std::chrono::duration<double,std::ratio <1,1000>>, because each millisecond is 1/1000th of a second
```

### Conversion
Conversion between durations is implicit where it does not require truncation of the value (so converting hours to seconds is OK, but converting seconds to hours is not). Explicit conversions can be done with std::chrono::duration_cast<>:
```
std::chrono::milliseconds ms(54802);
std::chrono::seconds s=
std::chrono::duration_cast<std::chrono::seconds>(ms);

The result is truncated rather than rounded, so s will have a value of 54 in this example.
```

### Time points
The time point for a clock is represented by an instance of the std::chrono::time_point<> class template, which specifies which clock it refers to as the first template parameter and the units of measurement (a specialization of std::chrono::duration<>) as the second template parameter
```
epoch: a specific point in time.
Unix epoch: 00:00 January 1, 1970
```
```
std::chrono::time_point<std:: chrono::system_clock, std::chrono::minutes>

Holding time relative to system_clock
```

### Waiting for a cv with a timeout
Recommended wait
```
#include <condition_variable>
#include <mutex>
#include <chrono>
std::condition_variable cv;
bool done;
std::mutex m;
bool wait_loop()
{
    auto const timeout= std::chrono::steady_clock::now()+
        std::chrono::milliseconds(500);
    std::unique_lock<std::mutex> lk(m);
    while(!done)
    {
        if(cv.wait_until(lk,timeout)==std::cv_status::timeout)
            break;
    }
    return done;
}
```
