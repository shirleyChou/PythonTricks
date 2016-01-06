# PythonTricks

Records of interesting implementations of Python.

***

### \_\_add\_\_

``` Python
>>> (54).__add__(21)
75
```



### Built-in Functions

#### sorted(List) & L.sort()

sort()是list的method，对于一个无序的列表a，调用a.sort()，对a进行排序后返回a。

``` Python
List.sort(cmp=None, key=None, reverse=False) 
```

sorted()是Python的built-in function，而对于同样一个无序的列表a，调用sorted(a)，对a进行排序后返回一个新的列表，而对a不产生影响。

``` Python
sorted(iterable, cmp=None, key=None, reverse=False) 
```

**参数**：  

* iterable：是可迭代类型；  
  
* cmp(e1, e2) 是带两个参数的比较函数， 返回值： 负数: e1 < e2, 0: e1 == e2， 正数： e1 > e2。 默认为 None， 即用内建的比较函数； 
  
* key(e) 是带一个参数的函数， **用来为每个元素提取比较值**。 默认为 None， 即直接比较每个元素；  
  
* reverse：排序规则。 reverse = True 或者 reverse = False，有默认值。  
  
* 返回值：是一个经过排序的可迭代类型，与iterable一样。  
  
  **注：一般来说，cmp和key可以使用lambda表达式。**  
  
  **注：通常, key 和 reverse 比 cmp 快很多, 因为对每个元素它们只处理一次; 而 cmp 会处理多次。**

**例子**：        

Sorting  cmp:

``` Python
>>>L = [('b',2),('a',1),('c',3),('d',4)]
>>>print sorted(L, cmp=lambda x,y:cmp(x[1],y[1]))
[('a', 1), ('b', 2), ('c', 3), ('d', 4)]
```

Sorting  keys:

``` Python
>>>L = [('b',2),('a',1),('c',3),('d',4)]
>>>print sorted(L, key=lambda x:x[1]))
[('a', 1), ('b', 2), ('c', 3), ('d', 4)]
```

如果我们想用第二个关键字排过序后再用第一个关键字进行排序：

``` Python
>>> L = [('d',2),('a',4),('b',3),('c',2)]
>>> print sorted(L, key=lambda x:(x[1],x[0]))
>>>[('c', 2), ('d', 2), ('b', 3), ('a', 4)]
```

Sorting  reverse:

``` Python
>>> print sorted([5, 2, 3, 1, 4], reverse=True)
[5, 4, 3, 2, 1]
>>> print sorted([5, 2, 3, 1, 4], reverse=False)
[1, 2, 3, 4, 5]
```

#### map() and reduce() and filter()

``` python
# map
def custom_sum(xs, transform):
    """Returns the sum of xs after a user specified transform."""
    return sum(map(transform, xs))

xs = range(5)
print custom_sum(xs, square)
print custom_sum(xs, cube)

# reduce
from operator import add
reduce(add, [1, 2, 3, 4, 5])   # 15

# filter
# filter(function, iterable) is equivalent to [item for item in iterable if function(item)]
a=[0,1,2,3,4,5,6,7]
filter(None, a)    # [1,2,3,4,5,6,7]

# precise usage of this three
from operator import add
expr = "28+32+++32++39"
print reduce(add, map(int, filter(bool, expr.split("+"))))
# 131
```

#### eval()

The eval function lets a python program run python code within itself.

``` python
>>> x = 1
>>> eval('x+1')   # 2

>>> a = "[[1,2], [3,4], [5,6], [7,8], [9,0]]" 
>>> b = eval(a)   # change string to list
>>> type(b)   # list

>>> a = "{1: 'a', 2: 'b'}"
>>> b = eval(a)   # change string to dict
>>> type(b)
```

#### sum()

``` python
>>> l = [[1,2,3], [4,5,6], [7], [8,9]]
>>> sum(l, [])
>>> [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

#### zip()

``` python
>>> a = [[1,2], [3,4], [5,6], [7,8], [9,0]]
>>> b = [8,7,9,7,9]
>>> c = zip(b, a)
>>> c   # [(8, [1, 2]), (7, [3, 4]), (9, [5, 6]), (7, [7, 8]), (9, [9, 0])]
>>> c.sort(key=lambda x:x[0])
>>> b, a = zip(*c)
>>> b   # (7, 7, 8, 9, 9)
>>> a   # ([3, 4], [7, 8], [1, 2], [5, 6], [9, 0])
```

#### iter()

**Iterators** represent streams of values. Because only one value is consumed at a time, they use very little memory. Use of iterators is very helpful for working with data sets too large to fit into RAM.

``` python
xs = [1,2,3]
x_iter = iter(xs)
for x in x_iter:
    print x
```

### Data Structure

#### String Magic
* str.replace(target, new)
  ``` python
  >>> a = "ABC defg"
  >>> print(a.replace('de','A'))   # ABC Afg
  ```

* str.startswith(str, beg=0, end=len(string))
  ```python
  # The method startswith() checks whether string starts with str, optionally restricting the matching with the given indices start and end.
  str = "this is string example....wow!!!";
  print str.startswith( 'this' )             # True
  print str.startswith( 'is', 2, 4 )         # True
  print str.startswith( 'this', 2, 4 )       # False
  
  ```

#### 用dict实现switch..case语法

**PHP中的switch..case语句**：

``` PHP
 switch ($value) {
     case 'a':
         $result = $x * 5;
         break;
     case 'b':
         $result = $x + 7;
         break;
     case 'c':
         $result = $x - 2;
         break;
 }
 ```
 **Python的等价实现为**：
 ```Python
 result = {
   'a': lambda x: x * 5,
   'b': lambda x: x + 7,
   'c': lambda x: x - 2
 }[value](x)
```

**例子**：

> 题目：Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

``` Python
def romanToInt(s):
    dic = {'I': lambda i: -1 if s[i + 1] in ['V', 'X'] else 1,
           'X': lambda i: -10 if s[i + 1] in ['L', 'C'] else 10,
           'C': lambda i: -100 if s[i + 1] in ['D', 'M'] else 100,
           'V': lambda i: 5,
           'L': lambda i: 50,
           'D': lambda i: 500,
           'M': lambda i: 1000}
    total = 0
    s += '@'
    for index, char in enumerate(s[:-1]):
        total += dic[char](index)
    return total
```

**解释**：      

1. char与dict.keys()对应，匿名函数接收one argument(which pass by index)，然后由匿名函数做出判断并返回一个integer。      
   
2. `为什么题目说保证input within the range from 1 to 3999？`  
   
   因为典型的罗马数字只有字母，超过4000的都要另外加非字母符号，例如小括号或者一横杠在字母上面。eg. (IV) = 4000



#### frozenset()

frozenset的作用是使传入的数据类型（无论是不是mutable）became immutable, 从而可以成为dictionary的键值（key）

``` Python
fs = frozenset(['a', 'b'])
print hash(fs)
# 316570333
```



#### collections

- defaultdict
- [Counter](http://www.pythoner.com/205.html)



### Comprehensions Magic

``` python
# create generator.
print (x*x for x in range(5))
# <generator object <genexpr> at 0x0000000002CC8510>

# create set
print {x for x in range(10)}
# set([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```



### Module

#### The `operator` module

The `operator` module provides “function” versions of common Python operators (+, *, [] etc) that can be easily used where a function argument is expected.

``` python
import operator as op

# Here is another way to express the sum function
print reduce(op.add, range(10))

# The pattern can be generalized
print reduce(op.mul, range(1, 10))
```



#### The `functools` module

The most useful function in the `functools` module is `partial`, which allows you to create a new function from an old one with some arguments “filled-in”

``` python
from functools import partial
import operator as op

sum_ = partial(reduce, op.add)
prod_ = partial(reduce, op.mul)
print sum_([1,2,3,4])
print prod_([1,2,3,4])

# This is extremely useful to create functions
# that expect a fixed number of arguments
from functools import partial

import scipy.stats as stats
import numpy as np

def compare(x, y, func):
    """Returne p-value for some appropriate comparison test."""
    return func(x, y)[1]

x, y = np.random.normal(0, 1, (100,2)).T

print "p value assuming equal variance    =%.8f" % compare(x, y, stats.ttest_ind)
test = partial(stats.ttest_ind, equal_var=False)
print "p value not assuming equal variance=%.8f" % compare(x, y, test)
```



#### The `itertools` module

This provides many essential functions for working with iterators.

The `permuations` and `combinations` generators may be particularly useful for simulations, and the `groupby` gnerator is useful for data analyiss.

``` python
from itertools import cycle, groupby, islice, permutations, combinations

print list(islice(cycle('abcd'), 0, 10))
print

animals = sorted(['pig', 'cow', 'giraffe', 'elephant',
                  'dog', 'cat', 'hippo', 'lion', 'tiger'], key=len)
for k, g in groupby(animals, key=len):
    print k, list(g)
print

print [''.join(p) for p in permutations('abc')]
print

print [list(c) for c in combinations([1,2,3,4], r=2)]
```



#### The `toolz`, `fn` and `funcy` modules

[for detail, visit](http://people.duke.edu/~ccc14/sta-663/FunctionsSolutions.html)

