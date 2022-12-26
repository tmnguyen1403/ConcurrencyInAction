```
 The compare-exchange operation is the cornerstone of programming with atomic types; it compares the value of the atomic variable with a supplied expected value and stores the supplied desired value if they’re equal.
  If the values aren’t equal, the expected value is updated with the value of the atomic variable. The return type of the compare-exchange functions is a bool, which is true if the store was performed and false otherwise. 
  The operation is said to succeed if the store was done (because the values were equal), and fail otherwise; the return value is true for success, and false for failure.
 ```