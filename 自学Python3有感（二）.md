# 自学Python3有感（二）

​    昨天晚上学习完之后，睡前回忆一天琐事，想到找完美数的过程，想到[在github上人家肯定给出了[源代码](https://github.com/jackfrued/Python-100-Days/blob/dependabot/pip/50WPython/code/Python/opencourse/lxml-4.6.2/Day01-15/code/Day05/perfect.py)，决定去看看，学习一下：

```python
"""
找出1~9999之间的所有完美数
完美数是除自身外其他所有因子的和正好等于这个数本身的数
例如: 6 = 1 + 2 + 3, 28 = 1 + 2 + 4 + 7 + 14
Version: 0.1
Author: 骆昊
Date: 2018-03-02
"""
import math

for num in range(1, 10000):
    result = 0
    for factor in range(1, int(math.sqrt(num)) + 1):
        if num % factor == 0:
            result += factor
            if factor > 1 and num // factor != factor:
                result += num // factor
    if result == num:
        print(num)
```

整个程序运行时间只有0.055s！

又震惊到我，在床上辗转反侧，难以入眠，遂一早就来编写有感（二）。

思考体会如下：

在我的优化思路中，对求余过程减半处理是我想到的结果。上述代码看完之后，因为完美数的定义，人家用算数平方根来界定循环的边界，而且在求余取整过程，又利用了因子的对称，减少循环次数。所以，我个人理解，这种优化思路本质上是一种**“去对称”**的过程。