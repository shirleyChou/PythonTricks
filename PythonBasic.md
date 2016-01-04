## PythonBasics

​Recap the basic knowledges of Python :smile:

---
#### String Magic
```python
>>> a = "ABC defg"
>>> print(a.replace('de','A'))   # ABC Afg

```
#### [Duck typing](http://stackoverflow.com/questions/4205130/what-is-duck-typing)

It is a term used in dynamic languages that do not have strong typing.

The idea is that you don't need a type in order to invoke an existing method on an object - if a method is defined on it, you can invoke it.



#### Decorators

This mechanism is useful for separating concerns and avoiding external un-relatedlogic ‘polluting’ the core logic of the function or method.     



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



#### Classes

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
  
  **Base Overloading Methods**
  
  | \_\_init__ (self [,args...]) | Constructor                              |
  | ---------------------------- | :--------------------------------------- |
  | **\_\_del__( self )**        | **Destructor**                           |
  | **\_\_repr__( self )**       | **Evaluatable string representation. Sample Call: repr(obj)** |
  | **\_\_str__( self )**        | **Printable string representation. Sample Call: str(obj)** |
  | **\_\_cmp__ ( self, x )**    | **Object comparison. Sample Call: cmp(obj, x)** |

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

        def __str__(self):
            # return the description of a class
            return '__str__: ' + LimitedStack.__name__

        def __repr__(self):
            return '__repr__: ' + LimitedStack.__name__

        def push(self, x):
            assert len(self.items) < self.limit
            super(LimitedStack, self).push(x)    # "super" method call

    f = FancyStack()
    l = LimitedStack(2)

    print hasattr(f, 'items')   # True   
    print hasattr(l, 'items')   # True
    print getattr(l, 'limit')   # 2
    # also delattr, setattr


    # docstring of the class
    print "LimitedStack.__doc__:", LimitedStack.__doc__     
    print "FancyStack.__dict__:", FancyStack.__dict__
    print "LimitedStack.__name__:", LimitedStack.__name__    # LimitedStack
    # Module name in which the class is defined. This attribute is "__main__" in interactive mode.
    print "LimitedStack.__module__:", LimitedStack.__module__   # __main__
    # Containing the base class
    print "LimitedStack.__bases__:", LimitedStack.__bases__


    # the function of __repr__ and __str__
    print repr(l)   # __repr__: LimitedStack
    print str(l)    # __str__: LimitedStack
    print l         # __str__: LimitedStack



    # data_hiding.py
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
  ```python
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
