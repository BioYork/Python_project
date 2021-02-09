# 自学python3有感 （一） 

  今日学习Python3，学到循环与判断部分[github](https://github.com/jackfrued/Python-100-Days/blob/master/Day01-15/05.%E6%9E%84%E9%80%A0%E7%A8%8B%E5%BA%8F%E9%80%BB%E8%BE%91.md)，练习题为找到10000以内的所有完美数，做出答案如下：

> 备注1：完美数又称为完全数或完备数，它的所有的真因子（即除了自身以外的因子）的和（即因子函数）恰好等于它本身。例如：6（$6=1+2+3$）和28（$28=1+2+4+7+14$）就是完美数。完美数有很多神奇的特性，有兴趣的可以自行了解。
>
> 文字源自：[github](https://github.com/jackfrued/Python-100-Days/blob/master/Day01-15/05.%E6%9E%84%E9%80%A0%E7%A8%8B%E5%BA%8F%E9%80%BB%E8%BE%91.md)
>
> 备注2：所用python版本为3.8.5

```python
for n in range(0, 10001):
    a = 0
    for i in range(1, n-1):
        if n % i == 0:
            a = a + i
    if n == a:
        print(n, " ")
```

运行结果如下：

```bash
0  
6  
28  
496  
8128
```

​    在上述程序运行过程中，有些许卡顿，是程序运行效率较低导致。想到在学R语言的时候，看李东风老师的R 语言教程有提到[程序效率](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/prog-prof.html)，其中提到“在执行迭代循环时， 效率较低， 与编译代码的速度可能相差几十倍”。python也是一种解释性语言，能否改变一些代码，使其运行速度提高一些？

​    仔细想下，再求余过程中没有必要从1开始到n-1去运算，只需到n/2即可，因为循环过半以后，再用n去除以该数，结果肯定介于1到2之间，并且不会整除，故优化如下：

```python
for n in range(0, 10001):
    a = 0
    for i in range(1, int((n / 2 + 1))):
        if n % i == 0:
            a = a + i
    if n == a:
        print(n, " ")
```

整体运行时间缩短不少，快了很多。

​    秉着学习的心态去百度一下，看看其他人的解法有何不同，看到一篇[答案](https://blog.csdn.net/lkm12138/article/details/100976568)，代码如下：

```python
def fun1(num):
    list1 = []
    for i in range(1, num):
        if num % i == 0:
            list1.append(i)
    return list1

for num in range(1, 10000):
    list2 = fun1(num)
    if sum(list2) == num:
        print(num, '是一个完数,约数有', list2)
```

人家的代码漂亮了很多，而且有用到列表和函数，但是也是有点卡顿，将求余过程减半，即变换第三行代码：

```python 
#变换前：
for i in range(1, num):
#变换后：
for i in range(1, int((num / 2 + 1))):    
```

运行效率简直是质的提升，几乎没有卡顿的感觉。调用python3 的time模块分别计算一下各个程序的运行时间：

```python 
import time

start = time.perf_counter()

#我的原始程序
for n in range(0, 10001):
    a = 0
    for i in range(1, n - 1):
        if n % i == 0:
            a = a + i
    if n == a:
        print(n, " ")

end = time.perf_counter()
print("花费时间为：%.3fs" % (end - start))

start = time.perf_counter()

#求余过程减半后
for n in range(0, 10001):
    a = 0
    for i in range(1, int((n / 2 + 1))):
        if n % i == 0:
            a = a + i
    if n == a:
        print(n, " ")

end = time.perf_counter()
print("花费时间为：%.3fs" % (end - start))

start = time.perf_counter()

#网上搜到他人解法的远程程序
def fun1(num):
    list1 = []
    for i in range(1, num):
        if num % i == 0:
            list1.append(i)
    return list1


for num in range(1, 10000):
    list2 = fun1(num)
    if sum(list2) == num:
        print(num, '是一个完数,约数有', list2)

end = time.perf_counter()
print("花费时间为：%.3fs" % (end - start))

start = time.perf_counter()

#求余过程减半后
def fun2(num):
    list1 = []
    for i in range(1, int((num / 2 + 1))):
        if num % i == 0:
            list1.append(i)
    return list1


for num in range(1, 10000):
    list2 = fun2(num)
    if sum(list2) == num:
        print(num, '是一个完数,约数有', list2)

end = time.perf_counter()
print("花费时间为：%.3fs" % (end - start))

```

结果如下：

```bash
0  
6  
28  
496  
8128  
花费时间为：3.357s
0  
6  
28  
496  
8128  
花费时间为：1.631s
6 是一个完数,约数有 [1, 2, 3]
28 是一个完数,约数有 [1, 2, 4, 7, 14]
496 是一个完数,约数有 [1, 2, 4, 8, 16, 31, 62, 124, 248]
8128 是一个完数,约数有 [1, 2, 4, 8, 16, 32, 64, 127, 254, 508, 1016, 2032, 4064]
花费时间为：2.146s
6 是一个完数,约数有 [1, 2, 3]
28 是一个完数,约数有 [1, 2, 4, 7, 14]
496 是一个完数,约数有 [1, 2, 4, 8, 16, 31, 62, 124, 248]
8128 是一个完数,约数有 [1, 2, 4, 8, 16, 32, 64, 127, 254, 508, 1016, 2032, 4064]
花费时间为：1.073s
```

我的原始程序运行时间长达3.3s，但是优化后只需1.6s，时间几乎缩短了一半！人家原先的解法需要时间2.1s，优化后是1s，时间整整缩短了一半还多！我有想到优化一些代码之后速度会快很多，但是快了将近一倍，着实惊讶到我了，也可能作为一个初学者，能看到这么大的变化有点奇怪。所以说，在学习过程中，尤其是循环，做一些思考，多查查资料，还是很有必要的。这只是很小很小的一块内容，如果放到生信组学中，自己不成熟的代码跟人家的比起来可能差的就不是几秒钟能衡量的。学无止境，共勉。



小插曲：

百度找到的第一篇关于time模块计算时间的函数是：

```python 
time.colck()
```

运行之后报错

```bash
AttributeError: module 'time' has no attribute 'clock'
```

仔细查看，time.colck()是Python3.6的方法函数，Python 3.8 已移除 clock() 方法 ，又去查看了Python文档，才找到time.perf_counter()。











