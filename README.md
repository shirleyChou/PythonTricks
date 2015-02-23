# PythonTricks
本文档收集编程时遇到的有意思的，高效率的，需要深思的，优美的Python写法。  
如有写错或交流的想法，欢迎Email me:shirleychou1@live.cn
***

## <a name="index"/>目录
* [__add__](#1)
* [sorted(List) & L.sort()](#2)
* [用dict实现switch..case语法](#3)

###<a name="1"/>__add__
```Python
>>> (54).__add__(21)
75
```
<br>

###<a name="2"/>sorted(List) & L.sort()
sort()是list的method，对于一个无序的列表a，调用a.sort()，对a进行排序后返回a。
```Python
List.sort(cmp=None, key=None, reverse=False) 
```
sorted()是Python的built-in function，而对于同样一个无序的列表a，调用sorted(a)，对a进行排序后返回一个新的列表，而对a不产生影响。
```Python
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
```Python
>>>L = [('b',2),('a',1),('c',3),('d',4)]
>>>print sorted(L, cmp=lambda x,y:cmp(x[1],y[1]))
[('a', 1), ('b', 2), ('c', 3), ('d', 4)]
```
Sorting  keys:
```Python
>>>L = [('b',2),('a',1),('c',3),('d',4)]
>>>print sorted(L, key=lambda x:x[1]))
[('a', 1), ('b', 2), ('c', 3), ('d', 4)]
```
>如果我们想用第二个关键字排过序后再用第一个关键字进行排序

```Python
>>> L = [('d',2),('a',4),('b',3),('c',2)]
>>> print sorted(L, key=lambda x:(x[1],x[0]))
>>>[('c', 2), ('d', 2), ('b', 3), ('a', 4)]
```
Sorting  reverse:
```Python
>>> print sorted([5, 2, 3, 1, 4], reverse=True)
[5, 4, 3, 2, 1]
>>> print sorted([5, 2, 3, 1, 4], reverse=False)
[1, 2, 3, 4, 5]
```
<br>

###<a name="3"/>用dict实现switch..case语法
PHP中的switch..case语句：
```PHP
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
 Python的等价实现为：
 ```Python
 result = {
   'a': lambda x: x * 5,
   'b': lambda x: x + 7,
   'c': lambda x: x - 2
 }[value](x)
```
例子：
>题目：Given a roman numeral, convert it to an integer.
Input is guaranteed to be within the range from 1 to 3999.

```Python
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
解释：      
1. char与dict.keys()对应，匿名函数接收one argument(which pass by index)，然后由匿名函数做出判断并返回一个integer。      
2. `为什么题目说保证input within the range from 1 to 3999？`  
因为典型的罗马数字只有字母，超过4000的都要另外加非字母符号，例如小括号或者一横杠在字母上面。eg. (IV) = 4000



