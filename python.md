# Python

## Configuration

```python
# .env file
DATABASE="my_db"

# app.py
from detenv import load_dotenv
import os
load_detenv()
database = os.getenv('DATABASE')
print(database)
```

## Virtual Environment

```bash
pip install virtualenv
virtualenv myenv
myenv/bin/activate
```

## time utils

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

## Unit Test

regression test

## classmethod and staticmethod

classmethod

* A class method is a method which is bound to the class and not the object of the class.
* They have the access to the state of the class as it takes a class parameter that points to the class and not the object instance.
* It can modify a class state that would apply across all the instances of the class. For example it can modify a class variable that will be applicable to all the instances.

staticmethod

* A static method is also a method which is bound to the class and not the object of the class.
* A static method can’t access or modify class state.
* It is present in a class because it makes sense for the method to be present in class.

```python
class Employee:
    num_of_emps = 0
    raise_amt = 1.04

    def __init__(self, first, last, pay):
        self.first = first
        self.last = last
        self.email = first + '.' + last + '@company.com'
        self.pay = pay

        Employee.num_of_emps += 1
        
    def __del__(self): 
        print("Destructor called") 
    
    def fullname(self):
        return '{} {}'.format(self.first, self.last)

    def apply_raise(self):
        self.pay = int(self.pay * self.raise_amt)

    @classmethod
    def set_raise_amt(cls, amount):
        cls.raise_amt = amount
        
    @classmethod
    def from_string(cls, emp_str):
        first, last, pay = emp_str.split('-')
        return cls(first, last, pay)
        
    @staticmethod
    def is_workday(day):
        if day.weekday() == 5 or day.weekday() == 6:
            return False
        return True
```

```bash
emp1 = Employee('Coery','Schafer',50000)
emp2 = Employee('Anny','Eager', 60000)

print(Employee.raise_amt)
print(emp1.raise_amt)
print(emp2.raise_amt)

Employee.set_raise_amt(1.05)

print(Employee.raise_amt)
print(emp1.raise_amt)
print(emp2.raise_amt)

new_emp_1 = Employee.from_string(emp1_str)
print(new_emp_1)

import datetime
target_date = datetime.date(2020,9,10)
print(Employee.is_workday(target_date))


dev_1 = Developer('Corey','Schafer',50000)

print(dev_1.pay)
dev_1.apply_raise()
print(dev_1.pay)
```

**Class method vs Static Method**

* A class method takes cls as first parameter while a static method needs no specific parameters.
* A class method can access or modify class state while a static method can’t access or modify it.
* In general, static methods know nothing about class state. They are utility type methods that take some parameters and work upon those parameters. On the other hand class methods must have class as parameter.
* We use @classmethod decorator in python to create a class method and we use @staticmethod decorator to create a static method in python.

**When to use what?**

* We generally use class method to create factory methods. Factory methods return class object \( similar to a constructor \) for different use cases.
* We generally use static methods to create utility functions.

## Inheritance

```python
class Developer(Employee):
    raise_amt = 1.10

    def __init__(self,first, last, pay):
        super().__init__(first, last, pay)
        
class Developer2(Employee):
  raise_amt = 1.30
  
  def __init__(self, first, last, pay):
    Employee.__init__(self, first, last, pay)
```

## Iterators

```python
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self

  def __next__(self):
    if self.a <= 20:
      x = self.a
      self.a += 1
      return x
    else:
      raise StopIteration

myclass = MyNumbers()
myiter = iter(myclass)

for x in myiter:
  print(x)
```

## Multi-threading

```python
from queue import Queue
from threading import Thread
import threading

class Worker(Thread):
    def __init__(self,tasks):
        Thread.__init__(self)
        self.tasks = tasks
        self.daemon = True
        self.start()
    
    def run(self):
        while True:
            func, args, kargs = self.tasks.get()
            try:    
                func(*args, **kargs)
            except Exception as e:
                print(e)
            finally:
                self.tasks.task_done()

class ThreadPool:
    def __init__(self, num_threads):
        self.tasks = Queue(num_threads)
        for _ in range(num_threads):
            Worker(self.tasks)

    def add_task(self, func, *args, **kargs):
        self.tasks.put((func,args,kargs))


    def map(self, func, args_list):
        """ Add a list of tasks to the queue """
        for args in args_list:
            self.add_task(func, args)

    def wait_completion(self):
        self.tasks.join()

if __name__ == '__main__':
    from random import randrange
    from time import sleep

    delays = [randrange(1,5) for i in range(100)]
    ticket = 0
    threadLock = threading.Lock()

    def wait_delay(d):
        global ticket
        # global threadLock
        
        threadLock.acquire()
        ticket += 1
        print("now ticket number is ",ticket)
        threadLock.release()
        sleep(d)

    pool = ThreadPool(20)

    pool.map(wait_delay,delays)

    pool.wait_completion()
```

##  

