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

