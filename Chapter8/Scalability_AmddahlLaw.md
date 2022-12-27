## Amdah's law formu
```
P = 1/(fs + 1-fs/N)
N : the number of processoer
fs : the fraction of serial part
P: the performance gain
```
Scalability is about reducing the time it takes to perform an action or increasing the amount of data that can be processed in a given time as more processors are added.
 
Sometimes these are equivalent (you can process more data if each element is processed faster), but not always. Before choosing the techniques to use for dividing work between threads, it’s important to identify which of these aspects of scalability are important to you.