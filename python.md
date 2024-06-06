[TOC]

本笔记主要用于记录python中多种多样的函数（~~太多了，记不过来脑袋要炸了~~）

# 对于字符串的函数、方法、操作

字符串是python中特殊类型，作为对象，在类中可以在字符串的后面加`.methodName`去调用字符串对象上的方法

```python
string="My Note"
```

## .lower()

将字符串中的字符全部转为小写

```python
string.lower()
```

```
my note
```

## .upper()

将字符串中字符全转为大写

```
string.upper()
```

```
MY NOTE
```

## .title()

将字符串中的字符转为标题格式（大概是每个单词首字母大写）

```
string.title()
```

```
My Note
```

## .split()

通过制定间隔符对字符串进行切片，并返回分割后的字符串列表

语法为：`.split(str='' '',num=string.cout(str))[n]`

参数说明：

- str:表示为分隔符，默认为空格，但是不能为空。若字符串中没有分隔符，则把整个字符串作为列表的一个元素

- num:表示分割次数。如果存在参数num，则仅分隔成 num+1 个子字符串，并且每一个子字符串可以赋给新的变量

- [n]表示选取第n个切片

```python
string.split()
stirng.split()[1]
s="www.cc.com"
s.split(.)#以"."为分隔符
s.split(.,2)#只分割两次
```

```
['My','Note']
Note
['www','cc','com']
['www','cc.com']
```

## .join()

```
' '.join(string)
```

```
'My Note'
```

## +

```
"0"+"1"
```

```
01
```

## *

```
"0"*3
```

```
'000'
```



# 对于列表的函数、方法、操作

初始化列表

```
a=[]
```

访问列表中的值可以在后面加`[]`

```
a[0]
```

## .count()

对列表中对象出现次数进行统计

```
a=[1,2,3,4,4,4,5]
print(a.count(4))
```

```
3
```

## .index()

从列表中找出某个值第一个匹配项的索引位置

语法：`list.index(x,start,end)`

参数说明：

- x：查找的对象
- start：可填可不填，是查找起始的位置
- end：可填可不填，是查找结束的位置

返回值：该方法返回查找对象的索引位置，如果没有找到对象则抛出异常。

## .sort()

对列表进行排序，这个方法会**直接修改原列表**，而不是返回一个新列表

语法：`list.sort(reverse=False)`

参数说明：

- 当`reverse=False`时，表示升序排序，如果不填参数默认为升序排序；当`reverse=True`，表示降序排序。

<u>相关的：python里有内置函数`sotred()`，这个函数也可以对任何可迭代对象进行排序，比如列表、元组、字典等。但该函数会返回一个新的排序的对象，不会修改原对象</u>

## .append()

在列表结尾添加一个对象

```
x=[1,2,3,4,5]
x.append(6)
print(x)
```

```
[1,2,3,4,5,6]
```

## .remove()

shanschu1









# 对于字典的函数、方法、操作

字典：将键(key)映射到值(value)的无序数据结构。值可以是任何值（列表，函数，字符串，任何东西）。键(key)必须是不可变的，例如，数字，字符串或元组。

```python
webstersDict = {'person': 'a human being, whether an adult or child', 'marathon': 'a running race that is about 26 miles', 'resist': ' to remain strong against the force or effect of (something)', 'run': 'to move with haste; act quickly'}
```

## 访问字典中的值

```python
webstersDict['marathon']
```

```
'a running race that is about 26 miles'
```

## 更新字典

```
webstersDict['shoe'] = 'an external covering for the human foot' #新加一个键和值
```