---
title: Python笔记
published: 2025-12-23
tags: [Python]
category: Technology
author: minai
draft: false
---

# 开始使用

## 安装原生`Python3.12.x`

1. 在python官网https://www.python.org/downloads选择合适的python版本，我这里是`Python3.12.7`
2. 安装时候，务必勾选`Use admin privileges when installing py.exe`以及`Add python.exe to PATH`
3. 使用`Pycharm`等工具进行开发

## 在PyCharm中新建项目

### 1.使用`Custom environment`(自定义环境)

1. 选择新建项目->自定义环境->生成新的
2. "从基础解释器继承软件包"，顾名思义就是从你总的解释器继承原来有的软件包和依赖，一般我们创建新的虚拟环境不会勾选
3. "可用于所有项目"表示在配置现有环境时或者设置解释器时会显示出该项目虚拟环境的路径

### 2.使用`Base coda`(基础 coda)

1. 将coda base环境配置为项目解释器

### 3.使用`Project venv`(项目 venv)

1. 根据选择的python基础解释器，在当前项目文件夹快速创建一个新的，不含其他依赖包的虚拟环境(venv)

下面是各个选项的具体含义

![img](Python笔记/5a2879f38d5d43b9ba85f3fa64ba0819.png)

# Python手册

## 变量命名

- 不含空格，不以数字开头，避开Python关键词和函数名如`print`

## 字符串

### 使用示例

`"这是一条字符串"` 

`'这也是一条字符串'`

 `"我叫'字符串'"`

 `'我也叫"字符串"'` 

`"this's a string"`



### 字符串方法

首字母大写`title()`

全部大写`upper()`

全部小写`lower()`

删除末端空白`rstrip()`

删除前端空白`lstrip()`

删除两端空白`strip()`

使用示例:

```py
name = "minai ice"
print(name.title())
print(name.upper())
```

输出:

```
Minai Ice
MINAI ICE
```



### 拼接字符串

使用示例:

```py
a = 'Minai'
b = 'ice'
name = a + b
print(name)
```

输出:

```(空)
Minai ice
```

### 使用函数`str()`转换非字符串变量



## 运算符

加`+`，减`-`， 乘`*`， 除`/`， 乘方`**`，地板除法`//`，求余`%`

按位与`&`, 按位或`` ` ``, 位异或`^`, 位取反`~`, 左移`<<`, 右移`>>`

## 列表

Python的列表比C语言的数组更加灵活, 甚至可以嵌套字典(类比C语言的结构体), 应该来说列表类似于C语言的结构体数组

### 使用示例

```py
oddnum = [1, 3, 5, 7]
print(oddnum[2])
print(oddnum[-1]) #在python中, 索引为负数表示倒数
```

```
5
7
```



### 列表方法

在末尾添加元素`append()`

插入元素至`insert(idx, variable)`



### 删除列表中的元素

#### 使用`del`语句

```py
oddnum = [1, 3, 5, 7]
del oddnum[0]
print(oddnum)
```

```
[3, 5, 7]
```

#### 使用`pop()`方法

```py
oddnum = [1, 3, 5, 7]
temp = oddnum.pop() #参数为索引值,留空表示删除最后一个元素
print(temp)
print(oddnum)
```

```
7
[1, 3, 5]
```

#### 使用`remove()`方法

```py
oddnum = [1, 3, 1, 2, 2]
oddnum.remove(1) #每调用一次, 删除最前面的目标元素
print(oddnum)
```

```
[3, 1, 2, 2]
```



### 列表排序

#### 使用`sort()`方法

```py
oddnum = [3, 7, 5, 1]
oddnum.sort()
print(oddnum)
oddnum.sort(reverse=True)
print(oddnum) #这种排序是永久性的
```

```
[1, 3, 5, 7]
[7, 5, 3, 1]
```

注: 列表元素为字符串时, 按照字母顺序排序

#### 使用`sorted()`函数

```py
oddnum = [3, 7, 5, 1]
print(sorted(oddnum, reverse=True))
print(oddnum) #这种排序是临时性的
```

```
[7, 5, 3, 1]
[3, 7, 5, 1]
```

#### 使用`reverse()`方法

```py
oddnum = [3, 7, 5, 1]
oddnum.reverse()
print(oddnum)
```

```
[1, 5, 7, 3]
```



### 获取列表长度

使用函数`len()`

```py
oddnum = [3, 7, 5, 1]
print(len(oddnum))
```

```
4
```



### 创建数字列表

```py
nums = list(range(1, 11, 2)) #使用函数range(begin, end, step), 输出不包含end值
print(nums)
```

```
[1, 3, 5, 7, 9]
```



### 简单的列表处理

- `min()`函数
- `max()`函数
- `sum()`函数
- `set()`函数



### 切片

创建切片

```py
nums = [1, 3, 5, 7, 9]
print(nums[1:])
print(nums[:3])
print(nums[-2:])
```

```
[3, 5, 7, 9]
[1, 3, 5]
[7, 9]
```

复制切片

```py
nums = [1, 3, 5, 7, 9]
tempnums = nums[:] #同时省略前后索引表示整个列表
print(tempnums)
```

```
[1, 3, 5, 7, 9]
```



## 元组

```py
nums = (1, 2, 3, 4, 5) #元组的元素为只读, 不可修改, 但可以给nums重新赋值一个新的元组
for i in num[:4]:
    print(i)
```

```
1
2
3
4
```



## 字典

### 使用示例

在Python中, 字典就是一系列`键-值`对, 我个人理解为类似C语言中的结构体:

```py
bA4 = {'freq': 440}
print(bA4['freq'])
```

```
440
```

不过Python字典比C语言结构体灵活, 例如可以添加新的`键-值`对:

```py
bA4['color'] = "white"
print(bA4['color'])
```

```
white
```

同理也可以删除`键-值`对:

```python
del bA4['color']
```

字典可以存字典, 也可以存列表

### 遍历字典

使用示例

```py
bA4 = {'freq': 440, 'color': "white"}
for key, val in bA4.items():
	print("key:" + key)
	print("value:" + str(val) + '\n')
```

```
key:freq
value:440

key:color
value:white

```

使用示例

```py
bA4 = {'freq': 440, 'color': "white"}
for key in bA4.keys(): #把keys()去掉, 效果相同
	print(key)
```

```
freq
color
```

同理, 用`values()`方法可以提取字典的值

此外, 遍历字典时并非严格按照顺序, 若有这个需求, 可以使用函数`sorted()`:

```py
user = {
    'name': "Minai",
    'account': '@minaiice',
    'password': '123456'
}
for k, v in sorted(user.items()): #按照key的首字母顺序
    print(k + ':' + v)
```

```
account:@minaiice
name:Minai
password:123456
```



## 获取用户输入

Python用`input()`函数获取的用户所有输入都解释为字符串, 如有需要则要进行转义:

```py
message = input("input a number:\n")
num = int(message)
if num == 1:
    print(num, "is not a prime or a composite number")
elif all(num % i != 0 for i in range(2, num)):
    print(num, "is a prime number")
else:
    print(num, "is a composite number")
```

```
input a number:
>>>37
37 is a prime number
```



## if语句

使用示例

```py
odds = []
for i in range(1, 11):
    if i % 2 != 0:
        odds.append(i)
print(odds)
```

```
[1, 3, 5, 7, 9]
```

使用示例

```py
prime_nums = []
for num in range(2, 101):
    for divisor in range(2, num):
        if num % divisor == 0:
            break
    else:
        prime_nums.append(num)
print(prime_nums)

#下面的写法更简洁
#prime_nums = [x for x in range(2, 101) if all(x % i != 0 for i in range(2, x))]
#print(prime_nums)
```

```
[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97]
```

使用示例

```py
prime_nums = [x for x in range(2, 101) if all(x % i != 0 for i in range(2, x))]
nums = list(range(1, 26))
for num in nums:
    if num in prime_nums:
        print(num)
```

```
2
3
5
7
11
13
17
19
23
```

与C语言相同, Python也拥有类似的`if else`结构, 即`if-elif-else`结构, 这里不再赘述, 此外, 在python中可以创建空的列表, 而空的列表会返回`False`, 非空列表会返回`True`

## 循环语句

### for循环

```py
nums = [1, 3, 5, 7, 10]
for num in nums: #num为临时变量
    print(num)
```

```
1
3
5
7
10
```

### while循环

与C语言基本相同:

```py
while True:
    print("give me two numbers, and I will check all prime numbers \nbetween the twos")
    num1 = int(input("the number_1:\n"))
    num2 = int(input("the number_2:\n"))
    if num1 <= 0 or num2 <= 0:
        print("the number is invalid")
        continue
    elif num1 >= num2:
        print("please make sure number_1 lower than number_2")
        continue
    else:
        nums = [x for x in range(num1, num2 + 1) if x > 1 and all(x % i != 0 for i in range(2, x))]
        if nums:
            print(nums)
        else:
            print("there are no prime numbers")
    message = 'y'
    while True:
        message = input("do you want to continue? (y/n)")
        if message.lower() == 'y' or message.lower() == 'n':
            break
    if message.lower() == 'n':
        break
```

```
give me two numbers, and I will check all prime numbers 
between the twos
the number_1:
>>>1
the number_2:
>>>37
[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37]
do you want to continue? (y/n)
>>>n
```

##  函数

### 定义函数

```py
def sqrt(val):
    if val < 0:
        return -1
    elif val == 0:
        return 0
    else:
        x = val
        while True:
            root = 0.5 * (x + val / x)
            if abs(root - x) < 0.001:
                return root
            x = root
```

### 函数传参

```py
def sum_squares(n2, n1=1): #n1可选, 默认为1
    if n1 < 0 or n2 < 0:
        return -1
    if n1 > n2:
        return 0
    x = 0
    for i in range(n1, n2 + 1):
        x += i * i
    return x
```

调用示例

```
>>>sum_squares(10)
385
>>>sum_squares(10, 2)
384
>>>sum_squares(n1=2, n2=10)
384
```

还可以传递任意数量的实参:

```py
def average(*numbers): #临时创建了一个numbers元组
    return sum(numbers) / len(numbers)
```

如果使用两个星号`**`表示接受任意数量`键-值`对，这里不再赘述



## 模块

将函数储存在模块中，使其与主程序分离，类似于C语言的`.c/.h`文件的关系

### 导入模块

#### 使用`import`语句

使用示例

```py
#math.py
def sqrt(val):
    if val < 0:
        return -1
    elif val == 0:
        return 0
    else:
        x = val
        while True:
            root = 0.5 * (x + val / x)
            if abs(root - x) < 0.001:
                return root
            x = root
```

```py
#main.py
import math

print(math.sqrt(2))
```

或者是仅导入特定函数:

```py
#main.py
from math import sqrt

print(sqrt(2))
```

也可导入模块中的所有函数:

```py
#main.py
from math imort *
```



#### 使用`as`语句给函数指定别名

```py
#main.py
from math import sqrt as Q

print(Q(2))
```



## 类

在Python中，首字母大写的名称一般是类，类中的函数称为方法

类和实例, 根据我的理解, 类是对某些特定对象的相同属性和功能的解释, 实例就是由类定义的具体对象

### 方法`__init__()`

每次根据类创建新示例时, Python都会自动运行这个方法, 其中必须包含形参self

使用示例

```py
class ImageHandler:
    def __init__(self, filename, width, height):
        self.filename = filename
        self.width = width
        self.height = height
        
    def resize(self, new_width, new_height):
        self.width = new_width
        self.height = new_height
        print(f"--> {self.filename} 已调整大小为 {self.width}x{self.height}")
```

### 创建实例

```py
img = ImageHandler("自拍.jpg", 1920, 1080)
```

### 调用方法

```py
img.resize(500, 500)
```

```
--> 自拍.jpg 已调整大小为 500x500
```

### 继承

一个类继承另一个类时, 它将自动获得另一个类的所有属性和方法; 原有的类称为父类, 而新类称为子类。子类继承了其父类的所有属性和方法，同时还可以定义自己的属性和方法:

```py
class AdvancedHandler(ImageHandler): #创建一个ImageHandler的子类
    def __init__(self, filename, width, height):
        super().__init__(filename, width, height) #用函数super()初始化父类的属性
        
	def addfilter(self, filter_name):
        print(f"--> {self.filename} 加上了 {filter_name} 滤镜")
```

此时, 子类能使用自己独特的功能外, 还可以使用父类的功能, 也可以重写父类的功能, 只需在子类定义处用相同名称覆写即可, 在此不再赘述

### 导入类

与从模块中导入函数的用法基本相同

使用示例

```py
from Image import ImageHandler
```

```py
from Image import ImageHandler, AdvancedHandler
```

```py
from Image import *
```

```py
import Image
```



## 文件操作

### 读取文件

使用示例

```py
with open('...\filename.txt') as file_object: #当文件与项目位于同一文件夹时, 可以不用写文件路径
    contents = file_object.read()
    print(contents)
```

```py
with open('filename.txt') as file_object:
	for line in file_object: #逐行读取
		print(line.rstrip()) #删除行末换行符
```

```py
filename = 'filename.txt'

with open(filename) as file_object:
    lines = file_object.readlines() #每一行都储存到lines列表中

for line in lines:
    print(line.rstrip())
```

### 调用`open()`的实参

第一个实参为文件名称和路径, 第二个为`'r'`(读取模式), `'w'`(写入模式), `'a'`(附加模式), `'r+'`(读取和写入模式), 默认值为读取模式

```py
filename = 'filename.txt' 
with open(filename, 'w') as file_object: #写入模式会将同名文件内容都清空, 若要在文末添加内容, 使用附加模式'a'
	file_object.write("hello world!\n") 
	file_object.write("I'm Minai\n") 
```



## 异常

在可能出现报错的地方添加异常处理逻辑, 可以防止无效输入和恶意攻击:

```py
try:
    answer = 5 / 0
except ZeroDivisionError:
    print("Eerror: divide by 0")
else:
    pass
```

















