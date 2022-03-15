---
title: 从4开始的C++世界生活（字符数组&字符串处理函数）
date: 2020-06-06 18:02:41
tags: 
 - C++
 - 入门
categories: 
 - C++
cover: /img/12.jpg
---

{% note info %}
本篇是学习VC驿站的《实用C++》教程时的超简化同步笔记，详细内容可参见VC驿站原帖
{% endnote %}

{% note warning %}
是在学校C++课程之外重新系统地学习一遍C++，所以很基础很基础的部分不会记录

代码块渲染有误，目前不知道怎么解决，#include后面的内容总是消失，所以#include统一写作包含，懂什么意思就行了

如果觉得网站字体太小，可以在右下方的按钮中点击+放大哦√（比如我就觉得这个网站的字体很小）

{% endnote %}

{% note success %}
希望对你和我都能有所帮助 : ）
{% endnote %}

# 字符数组

## 初始化方式

和一般一维数组初始化方式差不多，就多了个用字符串初始化的方式

1. 定义后，对每个字符初始化，未初始化的部分值随机。

   ```c++
   char szbuf[10];
   szbuf[0] = 'h';
   szbuf[1] = 'e';
   szbuf[2] = 'l';
   szbuf[3] = 'l';
   szbuf[4] = 'o';
   ```

2. 定义时初始化

   ```c++
   char szbuf[10] = {'h', 'e', 'l', 'l', 'o'};
   ```

   或

   ```c++
   char szbuf[10] = "hello";
   ```

   注意：

   1. 未初始化元素自动赋值为'\\0'（'\\0'的ASCII码的值为0）

   2. 若初始值的个数大于数组的长度，则直接报语法错误

      （可是我查百度时，说的是超出部分丢弃且没有字符串结尾符）

3. 自动初始化长度

   ```c++
   char szbuf[] = {'h', 'e', 'l', 'l', 'o', ' ', 'f', 'r', 'i', 'e', 'n', 'd', 's', ' ', 'I', ' ', 'a', 'm', ' ', 'c', 'c', 't', 'r', 'y', '.', 'c', 'o', 'm'};
   ```

   或

   ```c++
   char szbuf[] = "hello friends I am cctry.com";
   ```

## 字符数组Vs字符串

字符数组 + '\\0' = 字符串

是字符数组不是字符串：

```c++
char szbuf[5];
szbuf[0] = 'h';
szbuf[1] = 'e';
szbuf[2] = 'l';
szbuf[3] = 'l';
szbuf[4] = 'o';
```

是字符串：

```c++
char szbuf[6];
szbuf[0] = 'h';
szbuf[1] = 'e';
szbuf[2] = 'l';
szbuf[3] = 'l';
szbuf[4] = 'o';
szbuf[5] = '\0';
```

```c++
char szbuf[10] = {'h', 'e', 'l', 'l', 'o'};
```

```c++
char szbuf[10] = "hello";
```

 用字符串来进行初始化，所以会把字符串结束标志'\\0'一并初始化给字符数组szbuf 

## cin cout

```c++
char szbuf[50];
cin >> szbuf;
cout << szbuf << endl;
```

cin、cout是对字符串进行操作

所以如果定义了一个数组 char szbuf[5] = "hello"，用cout输出时，很有可能输出hello烫烫烫烫烫烫，找到'\\0'后才会停止输出

# 字符串处理函数

## 概述

提高开发效率的函数。已成为C/C++的标准——所有支持C/C++标准的编译器都支持这些函数。

包含在string.h/string头文件中，所以使用时要在代码中添加：

包含string.h //C语法
或
包含string //C++语法

## 连接：strcat

```c++
char * strcat (char destination[], const char source[]);
```

```c++
//eg.
char des[50] = "hello "; //保证des的空间足够大
char src[] = "cctry.com";
strcat(des, src);
cout << "des = " << des << endl;
```

1. 要保证第一个参数的字符数组大小够用

2. 返回值：第一个字符串的首地址

3. 在VS中使用时会报warning：让你使用函数strcat_s。

   这是微软提供的函数。和strcat的区别是，strcat_s会判断第一个数组的空间大小够不够，不够时会报警，更加安全。

## 拷贝：strcpy

```c++
char * strcpy( char destination[], const char source[]);
```

```c++
//eg.
char des[50] = {0};
char src[] = "cctry.com";
strcpy(des, src);
cout << "des = " << des << endl;
```

1. 要保证第一个参数的字符数组大小够用
2. 返回值：第一个字符串的首地址
3. 第2个参数的结束符 '\0' 也会拷贝过去 

## 比较：strcmp

```c++
int strcmp (const char str1[], const char str2[]);
```

```c++
//eg.
char des[50] = "hello";
char src[] = "cctry.com";
int iret = strcmp(des, src);
cout << "iret = " << iret << endl;
```

作用： 对比第1个和第2个参数的字符数组字符串，逐个字母比对，直到字符串结束。即比较每个字母的ASCII码值。当第1个参数大于第2个参数，返回 > 0 的数，当第1个参数小于第2个参数，返回 < 0 的数，当第1个参数和第2个参数相等，返回0 

## 求长度：strlen

```c++
size_t strlen (const char str[]);
```

```c++
//eg.
char des[50] = "hello";
int len = strlen(des);
cout << "len = " << len << endl;
//输出len = 5
```

长度不包括'\0'

## 求占用字节数：sizeof

int des[50];
sizeof(des) = 4 * 50 = 200

# 小作业：编写strcat函数

## 我的版本

```c++
char* my_strcat(char des[], const char src[])
{
	int len1 = strlen(des);
	int len2 = strlen(src);
	int i = 0, j = 0;
	for (i = len1; i <= len1 + len2; i++, j++)
	{
		des[i] = src[j];
	}
	des[i] = '\0';
	return des;
}

int main()
{
	char a[50] = "123456";
	char b[] = "abcdbbb";
	my_strcat(a, b);
	cout << a;
}
```

## 网上版本

包含stdio.h
包含assert.h

```c++
char* MyStrcat(char *dst, const char *src)
{
    assert(dst != NULL && src != NULL);   //表达式为0时报错
    char *temp = dst;    //因为要返回dst，所以dst不能改变，因此引入temp
    
    //很简洁
    
    //先让temp指向第一个字符串的'\0'处
    while (*temp != '\0')
        temp++;
    
    //连接
    while ((*temp++ = *src++) != '\0');    //同级运算符，结合方式查表知从右到左

    return dst;
}

int main()
{
    char str1[10] = "abc";
    char str2[] = "bcd";
    char* str3 = MyStrcat(str1, str2);

    printf("str1=%s\n", str1);
    printf("str2=%s\n", str2);
    printf("str3=%s\n", str3);
    return 0;
}
```

