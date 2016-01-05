## PythonBasics

​Recap the basic knowledges of Python :smile:

---

#### [Duck typing](http://stackoverflow.com/questions/4205130/what-is-duck-typing)

It is a term used in dynamic languages that do not have strong typing.

The idea is that you don't need a type in order to invoke an existing method on an object - if a method is defined on it, you can invoke it.

#### Generators
```python
# the creation of generator.
def fib():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a+b

for i in fib():
    # We must have a stopping condiiton since the generator returns an infinite stream
    if i > 1000:
        break
    print i,
# 0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
```

#### Decorators

Decorators are a type of HOF that take a function and return a wrapped function that provides additional useful properties.    

#### [Descriptor](https://docs.python.org/2/howto/descriptor.html?highlight=descriptor%20protocol)

and also: http://stackoverflow.com/questions/3798835/understanding-get-and-set-and-python-descriptors

#### Errors and Exceptions

##### assert

``` python
>>> assert False    # # raise AssertError if condition is false
AssertionError                            Traceback (most recent call last)
<ipython-input-23-6e6df518a476> in <module>()
----> 1 assert False
AssertionError:

# it’s roughly equivalent to:
>>>if not condition:
...  raise AssertionError()
```

#### Functions

In Python, functions are first class objects and behave like any other object, such as an int or a list. That means:

* you can use functions as arguments to other functions
* store functions as dictionary values
* return a function from another function

##### function arguments

what is passsed to the function is a copy of the name x that refers to the content (a list) `[1, 2, 3]`. 

``` python
def transmogrify(x):
    x[0] = 999
    return x

x = [1,2,3]
print x
print transmogrify(x)
print x
# [1, 2, 3]
# [999, 2, 3]
# [999, 2, 3]

def no_mogrify(x):
    x = [4,5,6]      
    return x

x = [1,2,3]
print x
print no_mogrify(x)
print x
# [1, 2, 3]
# [4, 5, 6]
# [1, 2, 3]
```

##### pure functions

Functions are pure if they do not have any side effects(likes changing the value of input) and do not depend on global variables. Pure functions are similar to mathematical functions - each time the same input is given, the same output will be returned.

``` python
def f1(x, y=[]):
    """Never give an empty list or other mutable structure as a default."""
    y.append(x)
    return sum(y)

# Here is the correct Python idiom

def f2(x, y=None):
    """Check if y is None - if so make it a list."""
    if y is None:
        y = []
    y.append(x)
    return sum(y)
```

##### Recursion function

A recursive function is one that calls itself. Recursive functions are extremely useful examples of the divide-and-conquer paradigm in algorithm development and are a direct expression of finite diffference equations. However, they can be computationally inefficient and their use in Python is quite rare in practice.

#### Classes

[wait to read](http://intermediatepythonista.com/classes-and-objects)

* **class variable**: a variable that is shared by all instances of a class. Classvariables are defined within a class but outside any of the class's methods.Class variables are not used as frequently as instance variables are.
* **instance variable**: A variable that is defined inside a method and belongs only to the current instance of a class.

``` python
class Connection(object):
    verbose = 0                                     # class variable

    def __init__(self, host):
        self.host = host                            # instance variable

    def debug(self, v):
        self.verbose = v                            # make instance variable

    def connect(self):
        if self.verbose:                            # class or instance variable?
            print "connection to", self.host
```

* **instance**: **An individual object of a certain class.** Anobject obj that belongs to a class Circle, for example, is an instance of theclass Circle.
* **object**: Aunique instance of a data structure that's defined by its class. An objectcomprises both data members (class variables and instance variables) andmethods.


* **[super()](http://stackoverflow.com/questions/222877/how-to-use-super-in-python)**
  
  The benefits of `super()` in single-inheritance are minimal -- mostly, you don't have to hard-code the name of the base class into every method that uses its parent methods.
  
  ``` python
  class Child(SomeBaseClass):
      def __init__(self):
          super(Child, self).__init__()
  
  # vs
  
  class Child(SomeBaseClass):
      def __init__(self):
          SomeBaseClass.__init__(self)
  ```


* **inheritance**


  ``` python
  # class_and_inheritance.py

  class Stack(object):
      "A well-known data structure..."
      def __init__(self):
          self.items = []

      def push(self, x):
          self.items.append(x)

      def pop(self):
          x = self.items[-1]
          del self.items[-1]
          return x

      def empty(self):
          return len(self.items) == 0


  class FancyStack(Stack): 
      """stack with added ability to inspect inferior stack items"""

      def peek(self, n):
          size = len(self.items)
          assert 0 <= n < size
          return self.items[size-1-n]


  class LimitedStack(FancyStack):  
      """fancy stack with limit on stack size"""

      def __init__(self, limit):
          # __init__ would overwrite parent class if no FancyStack.__init__(self)
          self.limit = limit
          super(LimitedStack, self).__init__()   # base class constructor

      def __del__(self):
          """Destructor"""
          pass

      def __str__(self):
          # return the description of a class
          return '__str__: ' + LimitedStack.__name__

      def __repr__(self):
          return '__repr__: ' + LimitedStack.__name__

      def __cmp__(self):
          """Object comparison. Sample Call: cmp(obj, x)"""
            pass

      def push(self, x):
          assert len(self.items) < self.limit
          super(LimitedStack, self).push(x)    # "super" method call

  f = FancyStack()
  l = LimitedStack(2)
  ```

* [object's properties](http://stackoverflow.com/questions/1251692/how-to-enumerate-an-objects-properties-in-python)
  
  ``` python
  # 1
  for property, value in vars(theObject).iteritems():
    print property, ": ", value
  """
  __module__ :  __main__
  __del__ :  <function __del__ at 0x0000000002D0BC88>
  __str__ :  <function __str__ at 0x0000000002D0BCF8>
  __cmp__ :  <function __cmp__ at 0x0000000002D0BDD8>
  __repr__ :  <function __repr__ at 0x0000000002D0BD68>
  push :  <function push at 0x0000000002D0BE48>
  __doc__ :  fancy stack with limit on stack size
  __init__ :  <function __init__ at 0x0000000002D0BC18>
  """
  
  # 2
  import inspect
  print [attr for attr, value in inspect.getmembers(LimitedStack)]
  """
  ['__class__', '__cmp__', '__del__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'empty', 'peek', 'pop', 'push']
  """
  ```
  
* [\_\_str\_\_ & \_\_repr\_\_](http://stackoverflow.com/questions/1436703/difference-between-str-and-repr-in-python)
  
  ``` python
  # the function of __repr__ and __str__
  print repr(l)   # __repr__: LimitedStack
  print str(l)    # __str__: LimitedStack
  print l         # __str__: LimitedStack
  ```
  
* built-in functions hasattr/getattr/delattr/setattr used in class    
  
  ``` python
  # these method can check whether instance variables exist or not
  print hasattr(f, 'items')   # True   
  print hasattr(l, 'items')   # True
  print getattr(l, 'limit')   # 2
  # also delattr, setattr
  ```
  
* [special attributes of functions and class](https://docs.python.org/2/reference/datamodel.html)


  ``` python
  # docstring of the class
  print "LimitedStack.__doc__:", LimitedStack.__doc__   
  # LimitedStack.__doc__: fancy stack with limit on stack size


  # pass
  print "FancyStack.__dict__:", FancyStack.__dict__
  # FancyStack.__dict__: {'peek': <function peek at 0x0000000002B3BBA8>, '__module__': '__main__', '__doc__': 'stack with added     ability to inspect inferior   stack items'}


  # Module name in which the class is defined. This attribute is "__main__" in interactive mode.
  print "LimitedStack.__name__:", LimitedStack.__name__    
  # LimitedStack.__name__: LimitedStack


  # pass
  print "LimitedStack.__module__:", LimitedStack.__module__   
  # LimitedStack.__module__: __main__


  # Containing the base class
  print "LimitedStack.__bases__:", LimitedStack.__bases__
  LimitedStack.__bases__: (<class '__main__.FancyStack'>,)
  ```

* data_hiding
  
  ``` python
  class JustCounter(object):
      __secretCount = 0    # private variable which is invisiable outside the class
  
      def count(self):
          self.__secretCount += 1
          print self.__secretCount
  
  counter = JustCounter()
  counter.count()                                       # 1
  counter.count()                                       # 2
  print counter._JustCounter__secretCount               # 2
  print counter.__secretCount                           # AttributeError
  ```
  
* [**@staticmethod vs @classmethod**](http://stackoverflow.com/questions/136097/what-is-the-difference-between-staticmethod-and-classmethod-in-python)
  
  * **With classmethods**, the class of the object instance is implicitly passed as the first argument instead of self. In fact, if you define something to be a classmethod, it is probably because you intend to call it from the class rather than from a class instance.
    
  * **With staticmethods**, neither self (the object instance) nor  cls (the class) is implicitly passed as the first argument. They behave like plain functions except that you can call them from an instance or the class
    
  * **self**, 指的是 instance. 也就是将实例本身作为第一个参数传递给函数。
    
    ![classmethod](https://github.com/shirleyChou/PythonTricks/blob/master/Res/trans-classmethod-staticmethod-1.png?raw=true)
  
  ``` python
  
  # encoding: utf-8
  
  class A(object):
      def foo(self,x):
          print "executing foo(%s,%s)"%(self,x)
  
      @classmethod
      def class_foo(cls,x):
          print "executing class_foo(%s,%s)"%(cls,x)
  
      @staticmethod
      def static_foo(x):
          print "executing static_foo(%s)"%x    
  
  a=A()
  
  a.foo(1)
  # executing foo(<__main__.A object at 0xb7dbef0c>,1)
  
  a.class_foo(1)
  # executing class_foo(<class '__main__.A'>,1)
  
  A.class_foo(1)
  # executing class_foo(<class '__main__.A'>,1)
  
  a.static_foo(1)
  # executing static_foo(1)
  
  A.static_foo('hi')
  # executing static_foo(hi)
  
  # when you call a.foo you don't just get the function, you get a "partially applied" version of the function 
  # with the object instance a bound as the first argument to the function. 
  print a.foo
  # <bound method A.foo of <__main__.A object at 0xb7d52f0c>>
  
  # With a.class_foo, a is not bound to class_foo, rather the class A is bound to class_foo
  print a.class_foo
  # <bound method type.class_foo of <class '__main__.A'>>
  
  # static_foo expects 1 argument, and a.static_foo expects 1 argument too.
  print(a.static_foo)
  # <function static_foo at 0xb7d479cc>
  ```
  
* [class descriptors](http://stackoverflow.com/questions/944592/best-practice-for-python-assert)
  
  ``` python
  class LessThanZeroException(Exception):
      pass
  
  class variable(object):
      def __init__(self, value=0):
          self.__x = value
  
      def __set__(self, obj, value):
          if value < 0:
              raise LessThanZeroException('x is less than zero')
  
          self.__x  = value
  
      def __get__(self, obj, objType):
          return self.__x
  
  class MyClass(object):
      x = variable()
  
  >>> m = MyClass()
  >>> m.x = 10
  >>> m.x -= 20
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "my.py", line 7, in __set__
      raise LessThanZeroException('x is less than zero')
  LessThanZeroException: x is less than zero
  ```
