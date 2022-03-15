---
title: 从2开始的C++世界生活（选择&循环语句）
date: 2020-06-01 21:05:31
tags: 
 - C++
 - 入门
categories: 
 - C++
cover: /img/6.jpg
---

{% note info %}
本篇是学习VC驿站的《实用C++》教程时的超简化同步笔记，详细内容可参见VC驿站原帖
{% endnote %}

{% note warning %}
是在学校C++课程之外重新系统地学习一遍C++，所以很基础很基础的部分不会记录

代码块渲染有误，目前不知道怎么解决，#include< iostream>总是消失，你们自己加上就好了QAQ

如果觉得网站字体太小，可以在右下方的按钮中点击+放大哦√（比如我就觉得这个网站的字体很小）

{% endnote %}

{% note success %}
希望对你和我都能有所帮助 : ）
{% endnote %}

# if语句

## 形式

### 类似switch语句

```c++
int zhangsan_age = 0;
cin >> zhangsan_age;

if (zhangsan_age < 1)
{
        cout << "张三是个婴儿" << endl;
}
else if (zhangsan_age < 3)
{
        cout << "张三是个幼儿" << endl;
}
else if (zhangsan_age < 5)
{
        cout << "张三是个儿童" << endl;
}
else if (zhangsan_age < 9)
{
        cout << "张三是个少年" << endl;
}
else if (zhangsan_age < 18)
{
        cout << "张三是个青年" << endl;
}
else if (zhangsan_age < 40)
{
        cout << "张三是个壮年" << endl;
}
else if (zhangsan_age < 55)
{
        cout << "张三是个中年人" << endl;
}
else
{
        cout << "张三是个老年人" << endl;
}
```



### 不带{}但意外的是正确的形式

（这种情况下if-else算作一条语句，编译能通过）

```c++
int zhangsan_age = 0, zhangsan_sex = 0;
cin >> zhangsan_age >> zhangsan_sex;

if (zhangsan_age < 18)
{
        if (zhangsan_sex == 1)
        {
                cout << "张三是个男孩" << endl;
        }
        else if (zhangsan_sex == 2)
        {
                cout << "张三是个女孩" << endl;
        }
}
else if (zhangsan_age >= 18)
        if (zhangsan_sex == 1)
                cout << "张三是个男青年" << endl;
        else if (zhangsan_sex == 2)
                cout << "张三是个女青年" << endl;
```

但是更建议使用嵌套写法（这样写好看）

```c++
int zhangsan_age = 0, zhangsan_sex = 0;
cin >> zhangsan_age >> zhangsan_sex;

if (zhangsan_age < 18)
{
        if (zhangsan_sex == 1)
        {
                cout << "张三是个男孩" << endl;
        }
        else if (zhangsan_sex == 2)
        {
                cout << "张三是个女孩" << endl;
        }
}
else if (zhangsan_age >= 18)
{
        if (zhangsan_sex == 1)
        {
                cout << "张三是个男青年" << endl;
        }
        else if (zhangsan_sex == 2)
        {
                cout << "张三是个女青年" << endl;
        }
}
```



# switch语句

## switch vs if-else

多条件时，使用if-else：

1. 代码冗余，不易阅读
2. 系统要从上到下一个个算if中的表达式进行比较，效率低（switch语句是直接跳转到匹配的case语句）

## 格式

```c++
switch(表达式)
{
    case 常量表达式1:语句1
    break;
    case 常量表达式2:语句2
    break;
    case 常量表达式3:语句3
    break;
    ...
    case 常量表达式n:语句n
    break;
    default:语句n+1;
    break;
}
```

### 具体说明

1. switch后面括号内的**表达式必须是数值类型（包括字符类型）**的数据，不能使用字符串；只能是整型数据，不能是浮点型数据。
2. 每个**case标签的常量表达式的值必须不能相同**，否则会出现错误的情况，而且vs2013的编译器来说编译也会不通过的。
3. 每个case语句，包括default语句的**顺序不影响结果**，可以任意放置，比如把default放在上面也是一样的。
4. 如果所有case都不匹配且没有default子句时，程序直接跳出switch-case。
5. break和return语句都能跳出switch-case，但是return更厉害会直接跳出函数



## 两个特殊用法

### 不同的case想要执行相同的语句

```c++
case 0:   //这里啥语句都没有，这一段放到最后面default之前
case 6:
    cout << "今天是双休日" << endl;
```

于是当x为0或6时，输出相同



### 想要在case中定义变量

不能直接在case/default的内嵌语句中定义局部变量，否则会报错。

两种解决方法：

1. 变量进switch语句之前就定义好

2. 给内嵌语句加一对大括号，限定作用域

   eg.

   ```c++
   char ch = 'A';
   switch(表达式)
   {
       case 'A':cout << "case A reach." << endl;
       break;
       case 'B':
       {
           int a = 6;
           ++a;
       }
       break;
       default:
       break;
   }
   ```

   

# 循环语句

循环语句没有什么特别要注意的地方

有意思的例题贴上来：

**求出100~200之间的素数并打印出来：** 

```c++
#include <iostream>
using namespace std;

int main()
{
        cout << "100~200之间的素数如下：" << endl;

        int x = 0;
        for (x = 100; x <= 200; ++x)
        {
                int y = 2;
                for (y = 2; y < x / 2; ++y)
                {
                        if (x % y == 0)
                        {
                                break;
                        }
                }

                if (y < x / 2) continue;

                cout << x << endl;
        }

        cout << endl;
        return 0;
}
```

第一次学C语言的时候，素数的题是用flag标志写出来的，这里用的是条件判断+continue

分析：里面的for循环要打破有两种方式：

1. x % y == 0，此时说明x不为素数
2. y >= x / 2

且条件1满足时，条件2必不满足（因为只有条件2不满足条件1才有执行的机会）

条件2满足时，条件1必不满足（因为条件2执行在条件1前面）

所以当x是素数时，y < x / 2 必成立

当x不为素数时，y < x / 2 必不成立



# 期中的题目

期中的题大多比较简单，这里记一个初学时写的不是很好的题：

> 输出以下图案：
> *
>
> \*\*\*
>
> \*\*\*\*\*
>
> \*\*\*\*\*\*\*
>
> \*\*\*\*\*
>
> \*\*\*
>
> \*

我自己写的代码：

```c++
for (int x = 0; x < 4; x++)
{
	for (int y = 0; y < 2 * x + 1; y++)
	{
		cout << "*";
	}
	cout << endl;
}

for (int x = 2; x >= 0; x--)
{
	for (int y = 0; y < 2 * x + 1; y++)
	{
		cout << "*";
	}
	cout << endl;
}
```

老师的代码：

```c++
for (int idx = 1; idx <= 7; idx += 2)
{
    for (int jdx = 0; jdx < idx; jdx++)
    {
        cout << '*';
    }
    cout << endl;
}

for (int idx = 5; idx >= 1; idx -= 2)
{
    for (int jdx = 0; jdx < idx; jdx++)
    {
        cout << '*';
    }
    cout << endl;
}
```

两种方式本质上是都用了 行号 和 要打印的*的个数 之间的数量关系写循环代码

但是老师的会更加好一点，因为它使得行号就是要打印的\*的个数，比我写的更简单直观，不用去算x和y之间的关系啥的（也就是顺序行号和要打印的\*的个数之间的关系）

虽然我的也是很简单的奇数的2x+1 =￣ω￣=



# VS零碎的基操

包含头文件：右键 -> 打开文件，可查看头文件内容（可以看到系统头文件标签页带把🔒，意思是只读）

缺少头文件时：小番茄会自己在缺少包含头文件的函数下面画小波浪，右键 -> Refactor(VA) -> Add Include，添加头文件，不用再去百度了



# blog零碎的基操

用markdown写博客时，有些字符比如说\*连打就会变成分割线，如果要打一串\*\*\*\*\*\*怎么办呢？

答案：加\\转义，比如打星号就是打\\\*