---
description: 'https://book.pythontips.com/'
---

# Python

## Configuration

```python
# .env file
DATABASE="my_db"

# app.py
from dotenv import load_dotenv
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
from datetime import datetime,timedelta
import pytz

# convert time string to time obj
absTime = time.strptime(start_time,'%Y-%m-%d-%H-%M-%S')  
# convert time obj to unix time stamp i.e.1584752096.0
start_time_stamp = mktime(absTime)
end_date = datetime.fromtimestamp(start_time_stamp + ttl_time).strftime("%Y-%m-%d:%H:%M:%S")

# convert current time to specific timezone
x = datetime.now()
x = x.astimezone(pytz.timezone("US/Pacific"))
print(x, x.year,x.month,x.day,x.hour,x.minute,x.second)

datetime_object = datetime.strptime('Jun 1 2005  1:33PM', '%b %d %Y %I:%M%p')
```

## Arguments

```python
import argparse

parser = argparse.ArgumentParser()
parser.add_argument("-v", "--verbose", help="increase output verbosity",
                    action="store_true")
parser.add_argument("pos", type=int,
                    help="position number")
parser.add_argument("--square", type=int,
                    help="display a square of a given number")
parser.add_argument("--filename", type=str,
                    help="input file name")

args = parser.parse_args()
if args.verbose:
    print("verbosity turned on")

if __name__  == "__main__":
    print(args.square)
    print(args.verbose)
    print(args.pos)
    print(args.filename)
```

```bash
$ python oa.py --square 33 4 --filename test
33
False
4
test


$ python oa.py -h
usage: oa.py [-h] [-v] [--square SQUARE] [--filename FILENAME] pos

positional arguments:
  pos                  position number

optional arguments:
  -h, --help           show this help message and exit
  -v, --verbose        increase output verbosity
  --square SQUARE      display a square of a given number
  --filename FILENAME  input file name
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

## Multi-processing

```python
from multiprocessing import Pool
from time import sleep

def f(x):
    return x*x

if __name__ == '__main__':
    # start 4 worker processes
    with Pool(processes=4) as pool:

        print "[0, 1, 4,..., 81]"
        print(pool.map(f, range(10)))

        # print same numbers in arbitrary order
        for i in pool.imap_unordered(f, range(10)):
            print(i)
```

```python
from multiprocessing import Process, Lock,Value

def func(l, i):
    l.acquire()
    try:
        ticket.value = ticket.value + 1
        print('hello world', ticket.value)
    finally:
        l.release()

if __name__ == '__main__':
    lock = Lock()
    ticket = Value('d', 0.0)
    for num in range(10):
        Process(target=func, args=(lock, ticket)).start()
```

## Files

```python
fout = open('output.txt', 'w')
count = 0
with open("flags.py",'r') as fp:  
    line = fp.readline()        
    while line:
        try:
            line = fp.readline()
            fout.write(line)
            count += 1
            fout.write('In %d years I have spotted %g %s.\n' % (3, count, 'camels'))
        except Exception as e:
            print(e)
fout.close()

#  string bytes key value only
import dbm
db = dbm.open('captions', 'c')
db['cleese.png'] = 'Photo of John Cleese.'
print(db['cleese.png'])


import pickle
t1 = [1, 2, 3]
s = pickle.dumps(t1)
t2 = pickle.loads(s)

pickle_out = open("myDB.pickle","wb")
pickle.dump(my_content, pickle_out)
pickle_out.close()

pickle_in = open("myDB.pickle","rb")
file_data = pickle.load(pickle_in)

import pandas as pd
df = pd.read_csv("mydata.csv")
print(df.columns)

import json
with open('mydata.json','r') as f:
    res = json.load(f)
```

## Pipes

```python
import os
file_name = "output.txt"
cmd = "md5sum " + file_name
fp = os.popen(cmd)
res = fp.read()
stat = fp.close()
print(res)
print(stat)
```

##  Copying

Shallow copy copies the object and any references it contains, but not the embedded objects.

Deep copy copies not only the object but also the objects it refers to, and the objects _they_ refer to, and so on.

```python
class Point:
    """Represents a point in 2-D space.

    attributes: x, y
    """


class Rectangle:
    """Represents a rectangle. 

    attributes: width, height, corner.
    """
    
box = Rectangle()
box.width = 100.0
box.height = 200.0
box.corner = Point()
box.corner.x = 50.0
box.corner.y = 50.0

import copy

# shallow copy
new_box = copy.copy(box)
print(new_box.corner is box.corner)  # True

# deep copy
new_box2 = copy.deepcopy(box)
print(new_box2.corner is box.corner)  # False
```

## Functions

**Pure function** does not modify any of the objects passed to it as arguments and it has no effect, like displaying a value or getting user input, other than returning a value.

**Modifiers** modify the objects it gets as parameters.

There is some evidence that programs that use pure functions are faster to develop and less error-prone than programs that use modifiers.

In general, I recommend that you write pure functions whenever it is reasonable and resort to modifiers only if there is a compelling advantage. This approach might be called a **functional programming style**.

```python
assert valid_time(t1) and valid_time(t2)
```

## magic variables

\*args is used to send a **non-keyworded** variable length argument list to the function 

\*\*kwargs allows you to pass **keyworded** variable length of arguments to a function. 

```python
def test_var_args(f_arg,*args,**kwargs):
    print(f_arg)
    for argv in args:
        print("arg value ", argv)
    for k,v in kwargs.items():
        print("{0} = {1}".format(k, v))
    print("-------")
    
pos_arg = "fixed arg"
list_args = ['A','B','C']
kv_args = {'1':'a','2':'b','3':'c'}
test_var_args(pos_arg,*list_args,**kv_args)
test_var_args(pos_arg,'T','R','T','R',k1=2,k2=3,k4=5)
```

```bash
>>  fixed arg
    arg value  A
    arg value  B
    arg value  C
    1 = a
    2 = b
    3 = c
    -------
    fixed arg
    arg value  T
    arg value  R
    arg value  T
    arg value  R
    k1 = 2
    k2 = 3
    k4 = 5
    -------
```

