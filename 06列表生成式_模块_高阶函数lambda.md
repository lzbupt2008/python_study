# 列表生成式

[TOC]

## 定义：

其实就是把‘迭代’和‘执行’（判断，选择），‘合并成列表’合成一步。`res =[x**2 for x in range(10) if(x%3)==0]  ` 

两个列表`res = [x*y for x in L1 for y in L2 if (x%3) ==0 if (y%2) == 0]`

两个变量 `d = {'1':'a', '2':'b','3':'c'} for i,j in d.items():print(i,'=',j)`

结果是1=a 2=b 3=c 列表生成式`s = [i+'='+j for i,j in d.items()]`

本质是取两个变量，1个是key，1个是value，只不过上面是从两个list里取，下边从一个里取，取出是一对一对的。

另外，字典也有`len`,`for i in len(d):print(d[i])`

# 模块

## `if __name__ == '__main__'`

每一个模块都有一个内置属性__name__。而__name__的值取决与python模块（.py文件）的使用方式。如果是直接运行使用，那么这个模块的__name__值就是“__main__”；如果是作为模块被其他模块调用，那么这个模块（.py文件）的__name__值就是该模块（.py文件）的文件名，且不带路径和文件扩展名。

## 私有函数

类似`_xxx`和`__xxx`这样的函数或变量就是非公开的（private），不**应该**被直接引用，比如`_abc`，`__abc`等；

之所以我们说，private函数和变量“不应该”被直接引用，而不是“不能”被直接引用，是因为Python并没有一种方法可以完全限制访问private函数或变量，但是，从编程习惯上不应该引用private函数或变量。

# 高阶函数



## 函数名也是变量

函数名其实就是指向函数的变量！对于`abs()`这个函数，完全可以把函数名`abs`看成变量，它指向一个可以计算绝对值的函数。可以对其进行修改，令其指向别处。如`abs = 10`，可想而知，这时abs已经成为赋值为10的变量，无法作为函数使用。

## 高阶函数

既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为*参数*，这种函数就称之为**高阶函数**。

一个最简单的高阶函数：

###### code 1 最简高阶

```python
def add(x, y, f):
    return f(x) + f(y)
```

当我们调用`add(-5, 6, abs)`时，参数`x`，`y`和`f`分别接收`-5`，`6`和`abs`，根据函数定义，我们可以推导计算过程为：`x = -5 y = 6 f = abs`即最后结果为`abs(-5) + abs(6)`

## map( )函数

`map()`函数接收两个参数，一个是函数，一个是`Iterable`对象。`map`的作用是将传入的函数依次作用到序列的每个元素，并把结果作为新的**`Iterator`**返回（感觉有点像列表生成式，把几个步骤组合起来）。如生成一个序列的平方，再生成为列表。

###### code 2 map() 函数

```python
def f(x): #定义平方函数
	return x*x
res = (map(f,range(10)))
#range(10)是Iterable的，和list，字典，元组一样
for value in res:
    print(value) #map返回的是迭代器，那就可以这么用了
#也可以形成一个list
print(list(res))
```

###### code 3 使用一行代码把数字list转成字符串list

`res=list(map(str,[1,2,3,4,5,5,6,345,35])) print(res)` >>>['1','2','3',....]

## reduce

`reduce`把一个函数作用在一个序列`[x1, x2, x3, ...]`上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算，其效果就是：`reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)`

###### code 4 将一个自然数序列连成一个整数

```python
def foo(x,y):
	return 10*x+y
L = [1,4,3,2,1]
res = reduce(foo, L)
print('The result is:',res) 
```

## filter

和`map()`类似，`filter()`也接收一个函数和一个序列。和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。关键在于正确实现一个“筛选”函数。

注意到`filter()`函数返回的是一个`Iterator`，也就是一个惰性序列，所以要强迫`filter()`完成计算结果，需要用`list()`函数获得所有结果并返回list，或用之前提到的`for i in res():pass`方法

###### code 5 判断是否为回文数字的filter实现

```python
def is_p(n):
	s = str(n) #转为字符串，才可以用切片
	temp = s[::-1] #利用切片生成倒叙字符串
	if s == temp:
		return True
	else: return False
output = filter(is_p,range(1000))#返回的就是生成器，直接用
for value in output:
	print(value)
print(list(output))#生成器用完，输出空list
#或用list(result)的方式
output2 = filter(is_p,range(1000))
print(list(output2))
```

## sorted 函数

`sorted()`函数也是一个高阶函数，它还可以接收一个key函数来实现自定义的排序，key指定的函数将作用于排序对象的每一个元素上，并根据key函数返回的结果进行排序。

###### code 6 sorted函数，对于元素的第二个值排序

```python
L = [('Bob',75),('Adam',92),('Bart',66),('Lisa',88)]
res = sorted(L,key=lambda x: x[1])
print(res)
```

这里的`lambda`的函数相当于

```python
def foo(x):
    return x[1]
```

## lambda 函数

匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数：