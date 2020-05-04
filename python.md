# Python

## Process time

```python
import time
from time import mktime
# convert time string to time obj
absTime = time.strptime(start_time,'%Y-%m-%d-%H-%M-%S')  
# convert time obj to unix time stamp i.e.1584752096.0
start_time_stamp = mktime(absTime)
```

## Decorator 

```python
def logger(func):
    def wrapper():
        print("Login...")
        func()
        print("Logout...")
    return wrapper

@logger
def myfunction():
    print("User operation")
```

> ```text
> >>> myfunction()
> Login... 
> User operation 
> Logout...
> ```

## Error Handler

```python
try:
    x = 1
    y = 1
    print(x/y)
except Exception as e:
    print("zero")
else:
    print("No error happened")
finally:
    print("Finally cleaned")
```

