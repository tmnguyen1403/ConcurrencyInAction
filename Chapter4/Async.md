### Listing 4.7 Passing arguments to a function with std::async
```
#include <string>
#include <future>
struct X
{
    void foo(int,std::string const&);
    std::string bar(std::string const&);
};
X x;
auto f1=std::async(&X::foo,&x,42,"hello");       1
auto f2=std::async(&X::bar,x,"goodbye");         2
struct Y
{
    double operator()(double);
};
Y y;
auto f3=std::async(Y(),3.141);                   3
auto f4=std::async(std::ref(y),2.718);           4
X baz(X&);
std::async(baz,std::ref(x));                     5
class move_only
{
public:
    move_only();
    move_only(move_only&&)
    move_only(move_only const&) = delete;
    move_only& operator=(move_only&&);
    move_only& operator=(move_only const&) = delete;
    void operator()();
};
auto f5=std::async(move_only());                 6
```
1 Calls p->foo(42,“hello”) where p is &x

2 Calls tmpx.bar(“goodbye”) where tmpx is a copy of x

3 Calls tmpy(3.141) where tmpy is move-constructed from Y()

4 Calls y(2.718)

5 Calls baz(x)

6 Calls tmp() where tmp is constructed from std::move(move_only())

