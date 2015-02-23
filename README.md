# PythonTricks
本文档收集编程时遇到的有意思的，高效率的，需要深思的，优美的Python写法。  
如有写错或交流的想法，欢迎Email me:shirleychou1@live.cn
***

## <a name="index"/>目录
* [__add__](#1)
* [sorted(List) & L.sort()](#2)
* [用dict实现switch..case语法](#3)

###<a name="1"/>__add__
>代码    

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





