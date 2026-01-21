---
title: NumPy手册
published: 2025-12-30
description: NumPy基础
tags: [Python]
category: Technology
author: minai
draft: false
---

# IPython基础

## 用符号`?`获取文档

```bat
In [3]: L = [1, 2, 3] 
In [4]: L.insert? 
Type:        builtin_function_or_method 
String form: <built-in method insert of list object at 0x1024b8ea8> 
Docstring:   L.insert(index, object) -- insert object before index
```

## 用符号`??`获取源代码

如果找不到源代码, 或者源代码不是用Python实现, 则效果与符号`?`相同

## `magic`命令

### 粘贴代码块: 使用`%paste` 和`%cpaste`

前者后输入并执行一个代码块, 后者打开一个交互式多行输入提示, 可以输入并执行多个代码块

### 执行外部代码: 使用`%run`

```bat
In [6]: %run filename.py
```

### 计算代码运行时间: 使用`%timeit`

单行代码

```bat
In [1]: %timeit L=[n ** 3 for n in range(1000)]
132 µs ± 612 ns per loop (mean ± std. dev. of 7 runs, 10,000 loops each)
```

多行代码

```bat
In [2]: %%timeit
   ...: L = []
   ...: for n in range(1000):
   ...:     L.append(n ** 2)
   ...:
137 µs ± 2.82 µs per loop (mean ± std. dev. of 7 runs, 10,000 loops each)
```

## 输入和输出历史

### 查询输出历史

上一个输出: `print(_)`

上上一个输出: `print(__)`

上上上一个输出: `print(___)`

倒数第n个输出: `Out[n]`或`_n`

所有输出历史: `%history`

### 禁止输出

在命令末尾添加分号, 程序仍然执行, 但不会存储至输出历史:

```bat
In [14]: math.sin(2) + math.cos(2);
In [15]: 14 in Out 
Out[15]: False
```



# NumPy笔记

## 创建一个NumPy数组

NumPy数组对比Python列表有一些限制:

- 数组的所有元素必须是相同类型的数据。
- 创建后，数组的总大小不能改变。
- 形状必须是“矩形”的，而不是“锯齿状”的；例如，二维数组的每一行必须具有相同的列数。

**在数学中，习惯上先用行索引再用列索引来引用矩阵的元素。这恰好适用于二维数组，但更好的心智模型是认为列索引在最后，行索引在倒数第二个。这可以泛化到具有任意维度数量的数组。**

Python列表的元素是一个包含数据和类型信息的结构体, 且可以被任意数据类型填充替换, 而NumPy式数组可以保证类型固定, 从而高效储存和操作数据:

```bat
In [1]: import numpy as np

In [2]: np.array([1, 3, 5, 7])
Out[2]: array([1, 3, 5, 7])

In [3]: np.array([1, 3.14, 3, 5])
Out[3]: array([1.  , 3.14, 3.  , 5.  ]
```

NumPy式数组元素类型必须相同, 如果数据类型不匹配, NumPy会先尝试向上转换(如整型变为浮点型)

指定数组类型:

```bat
In [5]: np.array([12, 34, 56, 78], dtype='uint32') #或者写作dtype=np.uint32
Out[5]: array([12, 34, 56, 78], dtype=uint32)
```

快速创建数组:

```bat
In [6]: np.zeros(10, dtype='int') #元素全为0
Out[6]: array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0])

In [7]: np.ones((3, 5), dtype='float') #元素全为1
Out[7]:
array([[1., 1., 1., 1., 1.],
       [1., 1., 1., 1., 1.],
       [1., 1., 1., 1., 1.]])

In [8]: np.full((2, 2), -1) #元素全为指定
Out[8]:
array([[-1, -1],
       [-1, -1]])
       
In [9]: np.arange(1, 20, 2) #从1开始到20结束(不含20), 步长为2
Out[9]: array([ 1,  3,  5,  7,  9, 11, 13, 15, 17, 19])

In [10]: np.linspace(0, 1, 5) #从0~1均匀分配, 等差数列
Out[10]: array([0.  , 0.25, 0.5 , 0.75, 1.  ])

In [11]: np.random.random((3, 3)) #0~1均匀分布的随机数
Out[11]:
array([[0.99154025, 0.73411449, 0.82374687],
       [0.42928839, 0.81169496, 0.17760098],
       [0.69531333, 0.86703552, 0.33086163]])

In [12]: np.random.normal(0, 1, (3, 3)) #均值为0, 方差为1, 正态分布的随机数
Out[12]:
array([[ 0.66172715, -1.72885572, -1.13889757],
       [ 0.0834858 , -0.10210823,  0.2300583 ],
       [ 0.83427514, -1.00449682,  1.70093234]])

In [13]: np.random.randint(0, 10, (3, 3)) #0~9的随机整数
Out[13]:
array([[8, 5, 1],
       [4, 4, 6],
       [9, 4, 6]])

In [14]: np.eye(3) #单位矩阵
Out[14]:
array([[1., 0., 0.],
       [0., 1., 0.],
       [0., 0., 1.]])

In [15]: np.empty(3) #未初始化数组, 数组值是内存空间的任意值
Out[15]: array([1., 1., 1.])
```

## NumPy的数据类型

| 数据类型   | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| bool_      | 布尔值（真、True或假、False），用一个字节存储                |
| int_       | 默认整型（类似于C语言中的long，通常情况下是int64或int32）    |
| intc       | 同C语言的int相同（通常是int32或int64）                       |
| intp       | 用作索引的整型（和C语言的ssize_t相同，通常情况下是int32或int64） |
| int8       | 字节（byte，范围从–128到127）                                |
| int16      | 整型（范围从–32768到32767）                                  |
| int32      | 整型（范围从–2147483648到2147483647）                        |
| int64      | 整型（范围从–9223372036854775808 到9223372036854775807）     |
| uint8      | 无符号整型（范围从0到255）                                   |
| uint16     | 无符号整型（范围从0到65535）                                 |
| uint32     | 无符号整型（范围从0到4294967295）                            |
| uint64     | 无符号整型（范围从0到18446744073709551615）                  |
| float_     | float64 的简化形式                                           |
| float16    | 半精度浮点型：符号比特位，5比特位指数（exponent）， 10比特位尾数（mantissa） |
| float32    | 单精度浮点型：符号比特位，8比特位指数，23比特位尾数          |
| float64    | 双精度浮点型：符号比特位，11比特位指数，52比特位尾数         |
| complex_   | complex128 的简化形式                                        |
| complex64  | 复数，由两个32位浮点数表示                                   |
| complex128 | 复数，由两个64位浮点数表示                                   |

## NumPy的数组属性

NumPy数组属性分为

- 数组维度`ndim`
- 数组每个维度的大小`shape`
- 数组大小`size`
- 数组的数据类型`dtype`
- 数组的元素字节大小`itemsize`
- 数组总字节大小`nbytes`

```bat
In [1]: import numpy as np
   ...: np.random.seed(0)
   ...: x1 = np.random.randint(10, size=6)
   ...: x2 = np.random.randint(10, size=(3, 4))
   ...: x3 = np.random.randint(10, size=(3, 4 ,5))
   
In [4]: x3
Out[4]:
array([[[8, 1, 5, 9, 8],
        [9, 4, 3, 0, 3],
        [5, 0, 2, 3, 8],
        [1, 3, 3, 3, 7]],

       [[0, 1, 9, 9, 0],
        [4, 7, 3, 2, 7],
        [2, 0, 0, 4, 5],
        [5, 6, 8, 4, 1]],

       [[4, 9, 8, 1, 1],
        [7, 9, 9, 3, 6],
        [7, 2, 0, 3, 5],
        [9, 4, 4, 6, 4]]])
   
In [5]: print('ndim:', x3.ndim)
   ...: print('size:', x3.size)
   ...: print('shape:', x3.shape)
   ...: print('dtype:', x3.dtype)
   ...: print('itemsize:', x3.itemsize, 'bytes')
   ...: print('nbytes:' , x3.nbytes, 'bytes')

ndim: 3
size: 60
shape: (3, 4, 5)
dtype: int32
itemsize: 4 bytes
nbytes: 240 bytes
```

## 数组索引与切片

### 索引

与Python列表索引基本一致, 而使用多维数组时, 可以用逗号分隔:

```bat
In [1]: import numpy as np
   ...: L = np.random.randint(10, size=(3,4))
   ...: L
Out[1]: 
array([[4, 7, 2, 3],
       [8, 8, 2, 5],
       [5, 8, 9, 8]])

In [2]: L[2,2]
Out[2]: 9

In [3]: L[:, 0] #取第一列
Out[3]: array([4, 8, 9])
```

高级索引:

```bat
In [1]: import numpy as np
   ...: x = np.random.randint(10, size=(3,4))
   ...: x
Out[1]: 
array([[1, 9, 7, 0],
       [4, 9, 3, 2],
       [2, 0, 8, 0]])

In [2]: x[:, [0,3]] #取第0列和第3列
Out[2]:
array([[1, 0],
       [4, 2],
       [2, 0]])
```



### 切片

与Python索引基本一致, 同样, 使用多维数组时, 可以用逗号分隔:

使用方式`x[start:stop:step]`

**前两个参数的默认值与`step`有关:当`step`取正值时, `start`和`stop`取`0`和`len(x)`;当`step`取负值时, `start`和stop取`len(x)-1`和`-1`(当然`-1`不是合法下标, 想要显式表达应该写作`None`)**(问就是方便倒数组)

```bat
In [2]: x1 = np.random.randint(10, size=6)
   ...: x2 = np.random.randint(10, size=(3,4))

In [3]: x1
Out[3]: array([1, 8, 6, 3, 8, 5])

In [4]: x2
Out[4]:
array([[3, 8, 6, 1],
       [6, 1, 5, 4],
       [1, 3, 5, 7]])

In [5]: x1[:3]
Out[5]: array([1, 8, 6])

In [6]: x1[::2]
Out[6]: array([1, 6, 8])

In [7]: x1[::-1] #倒数组, 相当于x1[len(x1)-1:None:-1]
Out[7]: array([5, 8, 3, 6, 8, 1])

In [8]: x2[:2, :3] #取前2行3列
Out[8]:
array([[3, 8, 6],
       [6, 1, 5]])

In [9]: x2[::, ::2] #取前2列
Out[9]:
array([[3, 6],
       [6, 5],
       [1, 5]])

In [10]: x2[::-1, ::-1] #倒二维数组
Out[10]:
array([[7, 5, 3, 1],
       [4, 5, 1, 6],
       [1, 6, 8, 3]])
```

## 获取特定元素

可以使用`np.nonzero()`获取满足条件的元素的索引, 使用`zip()`打包成元素的坐标

```bat
In [1]: import numpy as np
   ...: x = np.random.randint(10, size=(3,4))
   ...: x
Out[1]: 
array([[4, 2, 0, 5],
       [2, 9, 3, 9],
       [7, 0, 3, 6]])

In [2]: x[x<5] #返回x所有小于5的元素的一个数组
Out[2]: array([4, 2, 0, 2, 3, 0, 3])

In [3]: x<=5 #返回判断x元素不大于5的一个布尔值数组
Out[3]:
array([[ True,  True,  True,  True],
       [ True, False,  True, False],
       [False,  True,  True, False]])
       
In [4]: np.nonzero(x<=5) #返回行索引和列索引
Out[4]:
(array([0, 0, 0, 0, 1, 1, 2, 2], dtype=int64), #行索引
 array([0, 1, 2, 3, 0, 2, 1, 2], dtype=int64)) #列索引
 
In [5]: y=np.nonzero(x<=5) 
   ...: x[y] #与x[x<=5]效果相同
Out[5]: array([4, 2, 0, 5, 2, 3, 0, 3])

In [6]: z = list(zip(y[0], y[1])) #打包成一个对应元素的坐标列表
   ...: z
Out[6]: [(0, 0), (0, 1), (0, 2), (0, 3), (1, 0), (1, 2), (2, 1), (2, 2)]
```

## 副本和视图

NumPy切片与Python切片有一个很重要的不同: **一般情况下(非高级索引)NumPy数组切片返回的是视图而不是副本**, 这意味着你修改NumPy切片时, 原数组相应的值也将被相应修改:

```bat
In [1]: import numpy as np
   ...: x = np.random.randint(10, size=(4,4))
   ...: x_sub = x[:2, :2]
   ...: print(x)
   ...: print(x_sub)
[[0 2 0 1]
 [7 5 6 1]
 [7 6 6 6]
 [1 4 4 2]]
[[0 2]
 [7 5]]

In [2]: x_sub[0, 0] = 999
   ...: print(x_sub)
   ...: print(x)
[[999   2]
 [  7   5]]
[[999   2   0   1]
 [  7   5   6   1]
 [  7   6   6   6]
 [  1   4   4   2]]
```

所以, 真正创建副本的方式是使用方法`copy()`

```bat
In [3]: x_subcopy = x_sub.copy()
   ...: x_subcopy[0, 0] = 2233
   ...: print(x_sub)
[[999   2]
 [  7   5]]
```

通过高级索引创建的切片, 由于切片储存的内存不连续, 必定生成副本:

```bat
In [1]: import numpy as np
   ...: x = np.random.randint(10, size=(3,4))
   ...: x
Out[1]: 
array([[1, 9, 7, 0],
       [4, 9, 3, 2],
       [2, 0, 8, 0]])

In [2]: x[:, [0,3]]
Out[2]:
array([[1, 0],
       [4, 2],
       [2, 0]])

In [3]: x_subcopy = x[:, [0,3]]
   ...: x_subcopy[0, 0] = 999
   ...: print(x)
[[1 9 7 0]
 [4 9 3 2]
 [2 0 8 0]]
```

可以使用`base`属性可以快速查看创建的切片是视图还是副本, 视图会返回原始数组:

**`base`属性用来查看数组指向的内存, 如果一个数组自己就是内存所有者, 则返回`None`, 否则返回上一级数组**

```bat
In [1]: import numpy as np
   ...: x = np.arange(12)
   ...: x
Out[1]: array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])

In [2]: y = x.reshape(3,4)
   ...: y
Out[2]:
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])

In [3]: y.base #y.base is x
Out[3]: array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])

In [4]: z = y[:, [2,3]]
   ...: z
Out[4]:
array([[ 2,  3],
       [ 6,  7],
       [10, 11]])

In [5]: z.base #z与temp = y[:, [2,3]].T共享内存, 这是由于NumPy内部是先给temp划定内存再转置的
Out[5]:
array([[ 2,  6, 10],
       [ 3,  7, 11]])

In [6]: z.base is x #z是x的副本temp的一个视图,因此x与z不共享内存
Out[6]: False

In [7]: x.base is None #x本身为内存所有者, 返回True
Out[7]: True
```

## 数组操作

### 数组变形

- 使用`reshape()`方法, 变形后的数组与原数组大小一致, 通常生成视图, 但有时可能是副本
- 使用`newaixs`关键字, 增加一个维度, 实际上是`None`的别名

```bat
In [2]: x = np.arange(9)
   ...: x
Out[2]: array([0, 1, 2, 3, 4, 5, 6, 7, 8])

In [3]: x.reshape(3,3) #将某一维度填为-1, 则会自动计算
Out[3]:
array([[0, 1, 2],
       [3, 4, 5],
       [6, 7, 8]])

In [4]: x[np.newaxis,:] #创建一个1x9行向量, 效果与x[None, :]相同
Out[4]: array([[0, 1, 2, 3, 4, 5, 6, 7, 8]])

In [5]: x[:, np.newaxis] #创建一个9x1列向量, 效果与x[:, None]相同
Out[5]:
array([[0],
       [1],
       [2],
       [3],
       [4],
       [5],
       [6],
       [7],
       [8]])
```

`resize()`方法与`reshape()`方法使用类似, 但是`resize()`是直接将原数组重塑, 且不可逆

### 转置

- 使用`transpose()` / `T`

`T`属性可以快速转置一, 二维数组

```bat
In [2]: x = np.arange(4)
   ...: y = np.arange(6).reshape((2,3))
   ...: print(x)
   ...: print(y)
[0 1 2 3]
[[0 1 2]
 [3 4 5]]

In [3]: x.T
Out[3]: array([0, 1, 2, 3])

In [4]: y.T
Out[4]:
array([[0, 3],
       [1, 4],
       [2, 5]])
```

`transpose()`方法可以转置指定维度, 适用于高维数组

```bat
In [6]: z = np.random.randint(20, size=(2,3,4))

In [7]: z
Out[7]:
array([[[12,  8,  7,  7],
        [12, 15,  6,  7],
        [11,  8, 10, 19]],

       [[12,  5, 19,  0],
        [ 6, 11,  0,  8],
        [11,  7, 17, 10]]])
        
In [8]: z.transpose(1,0,2) #交换第0和第1个维度
Out[8]:
array([[[12,  8,  7,  7],
        [12,  5, 19,  0]],

       [[12, 15,  6,  7],
        [ 6, 11,  0,  8]],

       [[11,  8, 10, 19],
        [11,  7, 17, 10]]])
```

### 数组轴变换

- 使用`rollaxis()`函数, 持续滚动数组的某个轴到指定位置
- 使用`swapaxes()`函数, 交换数组的两个轴的维度

```bat
In [22]: a = np.arange(8).reshape((2,2,2))

In [23]: a
Out[23]:
array([[[0, 1],
        [2, 3]],

       [[4, 5],
        [6, 7]]])

In [24]: b = np.rollaxis(a, 2, 0) #将轴2滚动到轴0位置, 相当于a.transpose(2, 0, 1)

In [25]: b
Out[25]:
array([[[0, 2],
        [4, 6]],

       [[1, 3],
        [5, 7]]])

In [26]: c = np.swapaxes(a, 2, 0) #交换轴2和轴0的维度, 相当于a.transpose(2, 1, 0)

In [27]: c
Out[27]:
array([[[0, 4],
        [2, 6]],

       [[1, 5],
        [3, 7]]])
```



### 展平多维数组

- 使用`ravel()`方法
- 使用`flatten()`方法

```bat
In [13]: y.ravel() #生成视图
Out[13]: array([0, 1, 2, 3, 4, 5])

In [14]: y.flatten() #生成副本
Out[14]: array([0, 1, 2, 3, 4, 5])
```



### 数组拼接

- 使用`np.concatenate()`
- 使用`np.vstack()`(垂直栈)和`np.hstack()`(水平栈)
- **上述数组拼接方法生成的都必定是副本**

```bat
In [2]: x = np.array([1, 2, 3])
   ...: y = np.array([3, 2, 1])
   ...: np.concatenate([x, y])
Out[2]: array([1, 2, 3, 3, 2, 1])
   
In [3]: z = np.array([[1, 2, 2],
   ...:                [2, 3, 3]])
   ...: np.vstack([y, z]) #效果与np.concatenate([y, z], axis=1)相同
Out[3]:
array([[3, 2, 1],
       [1, 2, 2],
       [2, 3, 3]])

In [4]: y = np.array([[99],
   ...:             [99]])
   ...: np.hstack([z, y])
Out[4]:
array([[ 1,  2,  2, 99],
       [ 2,  3,  3, 99]])
```

### 数组分裂

- 使用`np.split()` , `np.vsplit()`(垂直分裂), `np.hsplit()`(水平分裂), `np.dsplit()`(纵深分裂)
- **规则分裂生成视图, 不规则会导致重新分配内存, 产生副本**

```bat
In [6]: x = np.array([1, 3, 5, 22, 33, 8, 9])

In [7]: x1, x2, x3 = np.split(x, [3, 5]) #列表内表示的是分裂点索引
   ...: print(x1, x2, x3)
[1 3 5] [22 33] [8 9]

In [8]: grid = np.arange(16).reshape((4, 4))
   ...: grid
Out[8]:
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15]])

In [9]: upper, lower = np.vsplit(grid, [2])
		print(upper) 
        print(lower) 
[[0 1 2 3] 
 [4 5 6 7]] 
[[ 8  9 10 11] 
 [12 13 14 15]] 

In [10]: left, right = np.hsplit(grid, [2])
    ...: print(left)
    ...: print(right)
[[ 0  1]
 [ 4  5]
 [ 8  9]
 [12 13]]
[[ 2  3]
 [ 6  7]
 [10 11]
 [14 15]]
```



## 数组计算

**在NumPy中除非万不得已, 不要使用Python循环来进行大量的计算, 这是非常慢且低效率的**

以下是提高算法运行效率的正确途径:

- 不使用Python自带的循环语句
- 减少局部, 碎片化的运算
- 使用NumPy的通用函数来代替Python的
- 减少临时副本的使用, 改为用out参数指定输出来代替
- 注意进行数组计算时不要触发dtype隐式转换

### 通用运算符

下面的运算符经过NumPy封装可以直接使用:

| 运算符 | 对应的通用函数  |
| :----: | :-------------: |
|   +    |     np.add      |
|   -    |   np.subtract   |
|   -    |   np.negative   |
|   *    |   np.multiply   |
|   /    |    np.divide    |
|   //   | np.floor_divide |
|   **   |    np.power     |
|   %    |     np.mod      |
|   &    | np.bitwise_and  |
|   \|   |  np.bitwise_or  |
|   ^    | np.bitwise_xor  |
|   ~    | np.bitwise_not  |

### 数学函数

绝对值:

- 对应的NumPy通用函数是`np.absolute()`，该函数也可以用别名`np.abs()`来访问
- 处理复数时, 返回模长

三角函数:

- 正弦`np.sin()`, 余弦`np.cos()`, 正切`np.tan()`
- 反正弦`np.arcsin()`, 反余弦`np.arccos()`, 反正切`np.arctan()`/`np.atan()`
- 反正切并选择对应象限`np.arctan2()`/`np.atan2()`
- 给定直角边返回斜边`np.hypot()`
- 弧度转角度`np.degrees()`/`np.rad2deg()`
- 角度转弧度`np.radians()`/`np.deg2rad()`

指数和对数：

- 指数`np.exp(x)`(以`e`为底), `np.exp2(x)`(以`2`为底), `np.power(n, x)`(以`n`为底)
- 对数`np.log(x)`(以`e`为底), `np.log2(x)`(以`2`为底), `np.log10(x)`(以`10`为底)
- `np.expm1(x)`(`exp(x) - 1`的特殊封装, 精度更高)
- `np.log1p(x)`(`log(1 + x)`的特殊封装, 精度更高)

**更多数学函数参考官方文档给出的API参考**

### 指定输出

在NumPy的通用函数中, 可以使用`out`参数指定计算结果的存放位置:

```bat
In [2]: x = np.arange(5)
   ...: y = np.empty(5) #初始化一个y
   ...: np.multiply(x, 5, out=y)
   ...: y
Out[2]: array([ 0.,  5., 10., 15., 20.])

In [3]: y = np.zeros(10)
   ...: np.exp2(x, out=y[::2]) #在y中每隔一个元素放一个结果
   ...: y
Out[3]: array([ 1.,  0.,  2.,  0.,  4.,  0.,  8.,  0., 16.,  0.])
```

对于较大的数组, 合理使用`out`参数可以有效节约内存

### 聚合运算

我们可以对NumPy的通用函数使用一个`reduce`方法, 实现聚合运算效果

`accumulate`方法也是聚合运算, 不过它会累计每个结果到一个列表, 而`reduce`只会返回一个结果:

```bat
In [4]: x = np.arange(1, 6)
   ...: np.add.reduce(x)
Out[4]: 15

In [5]: np.multiply.accumulate(x)
Out[5]: array([  1,   2,   6,  24, 120])
```

使用NumPy的聚合函数(如`sum`, `min`, `max`等)时, 可以直接对数组对象调用方法, 例如`L.sum()`此时程序执行的仍然是NumPy版本的聚合函数

聚合函数还有一个`axis`参数, 可以指定聚合的方向:

```bat
In [8]: x
Out[8]:
array([[0, 1, 2],
       [3, 4, 5],
       [6, 7, 8]])

In [10]: x.sum(axis=0) #按列求和(延行方向操作)
Out[10]: array([ 9, 12, 15])
```

**`axis`关键字指定的是数组将会被折叠的维度, 而不是将要返回的维度, 一维数组第0个维度是列, 而二维数组第0个维度是行, 第1个维度是列**

其他聚合函数:

|   函数名称    |   NaN安全版本    |           描述           |
| :-----------: | :--------------: | :----------------------: |
|    np.sum     |    np.nansum     |       计算元素的和       |
|    np.prod    |    np.nanprod    |       计算元素的积       |
|    np.mean    |    np.nanmean    |     计算元素的平均值     |
|    np.std     |    np.nanstd     |     计算元素的标准差     |
|    np.var     |    np.nanvar     |      计算元素的方差      |
|    np.min     |    np.nanmin     |        找出最小值        |
|    np.max     |    np.nanmax     |        找出最大值        |
|   np.argmin   |   np.nanargmin   |     找出最小值的索引     |
|   np.argmax   |   np.nanargmax   |     找出最大值的索引     |
|   np.median   |   np.nanmedian   |     计算元素的中位数     |
| np.percentile | np.nanpercentile | 计算基于元素排序的统计值 |
|    np.any     |       N/A        | 验证任何一个元素是否为真 |
|    np.all     |       N/A        |   验证所有元素是否为真   |

使用NaN版本的聚合时, 自动忽略缺失值(即NaN值)继续运算, 防止NaN值污染

### 外积

我们可以对NumPy的通用函数使用outer方法, 对两个不同数组的所有元素对都进行一次运算:

```bat
In [6]: x = np.arange(1, 6)
   ...: np.multiply.outer(x,x)
Out[6]:
array([[ 1,  2,  3,  4,  5],
       [ 2,  4,  6,  8, 10],
       [ 3,  6,  9, 12, 15],
       [ 4,  8, 12, 16, 20],
       [ 5, 10, 15, 20, 25]])
```



### 广播

广播机制可以使得不同维数的数组可以进行运算:

```bat
In [2]: x = np.random.randint(10, size=(2,3))
   ...: y = 5
   ...: x
Out[2]:
array([[3, 0, 9],
       [3, 0, 3]])

In [3]: x + y #与一个标量(零维数组)相加, y被拓展成2x3数组
Out[3]:
array([[ 8,  5, 14],
       [ 8,  5,  8]])
       
In [4]: a = np.arange(3)
   ...: M = np.ones((3,3))
   ...: a + M #a从1x3拓展到3x3数组
Out[4]:
array([[1., 2., 3.],
       [1., 2., 3.],
       [1., 2., 3.]])

In [5]: b = np.arange(3)[:,None]
   ...: print(a)
   ...: print(b)
[0 1 2]
[[0]
 [1]
 [2]]

In [6]: a + b #a, b拓展成3x3数组
Out[6]:
array([[0, 1, 2],
       [1, 2, 3],
       [2, 3, 4]])
```

![](NumPy手册/20260103004131961.png)

广播遵循以下规则:

- 如果两个数组的维度数不相同, 那么小维度数组的形状将会在最左边补1
- 如果两个数组的形状在任何一个维度上都不匹配, 那么数组的形状会沿着维度 为1的维度扩展以匹配另外一个数组的形状
- 如果两个数组的形状在任何一个维度上都不匹配并且没有任何一个维度等于1, 那么会引发异常

```bat
In [2]: a = np.arange(3).reshape((3,1))
   ...: b = np.arange(3)
   ...: print(a.shape)
   ...: print(b.shape)
(3, 1) #-> (3, 1)-> (3, 3)
(3,)   #-> (1, 3)-> (3, 3)
       #最终数组兼容, 可以广播
In [5]: a + b 
Out[5]:
array([[0, 1, 2],
       [1, 2, 3],
       [2, 3, 4]])
       
In [6]: M = np.ones((3,2))
   ...: print(M.shape)
   ...: print(b.shape)
(3, 2) #-> (3, 2)-> (3, 2)
(3,)   #-> (1, 3)-> (3, 3)
       #最终数组不兼容, 无法广播
In [7]: M + b
```



## 数组排序

使用NumPy中的排序函数对数组进行排序效率会比Python自带的排序函数更高:

- `np.sort()`返回排序好数组的副本, 默认使用快速排序, 可以通过参数设置为归并排序和堆排序
- `np.argsort()`返回已排序的索引值

```bat
In [2]: x = np.array([2, 1, 4, 3, 5])
   ...: np.sort(x)
Out[2]: array([1, 2, 3, 4, 5])

In [4]: np.argsort(x)
Out[4]: array([1, 0, 3, 2, 4], dtype=int64)
```

参数`axis`可以指定排序的维度:

```bat
In [7]: y = np.random.randint(1, 10, size=(4,4))

In [8]: y
Out[8]:
array([[4, 2, 1, 3],
       [4, 6, 6, 3],
       [7, 6, 4, 8],
       [2, 1, 8, 3]])

In [9]: np.sort(y, axis=0) #按行方向排序, 即每列独自排序
Out[9]:
array([[2, 1, 1, 3],
       [4, 2, 4, 3],
       [4, 6, 6, 3],
       [7, 6, 8, 8]])

In [10]: np.sort(y, axis=1) #按列方向排序, 即每行独自排序
Out[10]:
array([[1, 2, 3, 4],
       [3, 4, 6, 6],
       [4, 6, 7, 8],
       [1, 2, 3, 8]])
```

当我们不想对整个数组进行排序时, 只是希望找到数组最小的K个值, 可以使用函数`np.partition()`

```bat
In [19]: x = np.random.randint(20, size=15)

In [20]: x
Out[20]: array([ 9, 12,  3, 14, 19,  7,  5, 16,  8,  4, 16,  1, 19, 19,  1])

In [21]: np.partition(x, 3)
Out[21]: array([ 1,  1,  3,  4,  5,  7,  8,  9, 12, 19, 16, 14, 19, 19, 16])
```

注意, 此时返回的数组前K个数是最小的K个值, 而无论是前K个目标值还是剩余的元素都是没有经过排序的

当然想返回目标数组的索引可以使用`np.argpartition()`, 也可设置`axis`参数对多维数组作用







