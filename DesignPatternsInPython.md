* why the design patterns in Python is lack?
  * The patterns are built in


* The language features of Python:
  * First-class functions
  * Meta-programming
    * The idea that class is the first class of object.
  * Iterators
  * Closures



``` python
# Observer Pattern in Python

class A(object):
    def f(self):
        return "Hello"

def wrap_f(f):
    def _(self):
        print "Called f"
        return f(self)
    return _


A.f = wrap_f(A.f)
print A.f.__name__  # _
a = A()
print a.f() 
# Called f
# Hello
```

``` python
# PyCSP - https://code.google.com/p/pycsp/

from common import *
from pycsp import *

@process
def producer(cout):
    for i in range(2, 10000):
        cout(i)
    poison(cout)

@process
def printer(cin):
    while True:
        print cin()

@process
def worker(cin, cout):
    try:
        ccout = None
        my_prime = cin()
        cout(my_prime)
        child_channel = Channel()
        ccount = OUT(child_channel)
        Spawn(worker(IN(child_channel), cout))
        while True:
            new_prime = cin()
            if new_prime % my_prime:
                ccout(new_prime)
    except ChannelPoisonException:
        if ccout:
            poison(ccout)
        else:
            poison(cout)
```

