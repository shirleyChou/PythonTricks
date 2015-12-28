## PythonBasics

​Recap the basic knowledges of Python :smile:

---

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

* **@staticmethod vs @classmethod**
  
  ``` python
  
  # encoding: utf-8

  class Kls(object):

``` 
  def __init__(self, data):
      self.data = data

  def printd(self):
      print self.data

  @staticmethod
  def smethod(*arg):
      print('Static:', arg)

  @classmethod
  def cmethod(*arg):
      print('Class:', arg)
```

  ik = Kls(23)

  ik.printd()    # 23

  ik.smethod()   # ('Static:', ())

  Kls.printd()   # TypeError: unbound method printd() must be called with Kls instance as first argument

  ik.cmethod()   # ('Class:', (<class '__main__.Kls'>,))

  Kls.smethod()   # ('Static:', ())

  Kls.cmethod()   # ('Class:', (<class '__main__.Kls'>,))

  ```
<<<<<<< HEAD

图解：

![2](https://github.com/shirleyChou/PythonTricks/blob/master/Res/trans-classmethod-staticmethod-2.png?raw=true)



=======
  
>>>>>>> 984c3d83ceba0bdea163b72ba905eeffd6730ab9
* **self**
  
  self 指的是 instance. 也就是将实例本身作为第一个参数传递给函数。
  
  ![classmethod](https://github.com/shirleyChou/PythonTricks/blob/master/Res/trans-classmethod-staticmethod-1.png?raw=true)
  
* 飞电风扇
