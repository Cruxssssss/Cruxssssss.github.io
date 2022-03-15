---
title: 从7开始的C++世界生活（枚举&typedef&VS实用）
date: 2020-06-21 14:04:57
tags: 
 - C++
 - 入门
categories: 
 - C++
cover: /img/16.jpg
---

{% note info %}
本篇是学习VC驿站的《实用C++》教程时的超简化（不太会的部分会写多一点）同步笔记，详细内容可参见VC驿站原帖
{% endnote %}

{% note warning %}
是在学校C++课程之外重新系统地学习一遍C++，所以很基础很基础的部分不会记录

代码块渲染有误，目前不知道怎么解决，#include后面的总是消失，所以#include统一写作包含，懂什么意思就行了

如果觉得网站字体太小，可以在右下方的按钮中点击+放大哦√（比如我就觉得这个网站的字体很小）

{% endnote %}

{% note success %}
希望对你和我都能有所帮助 : ）
{% endnote %}

# 枚举类型

## 概念

如果一个变量只能有几种可能的值，这样的变量可以定义成枚举类型。所谓的 “枚举” 是指可以将变量的值一一列举出来，变量的值只能在列举出来的值的范围内，其他的值对该变量没有意义，例如：星期几、人种颜色、性别、月份 等等。**枚举类型也是一种自定义类型。** 

## 声明

enum ESex
{
    ESex_Male, //男性
    ESex_FMale //女性
};

以上就是定义了一个**枚举类型** ESex，大括号内部的ESex_Male、ESex_FMale 称为**枚举元素或枚举常量**。表示这个枚举类型可能的值。

注意事项：

1. 枚举元素按常量处理，所以称作枚举常量。他们不是变量，所以不要对他们进行赋值。

2. 枚举元素是常量，所以其也是有值的，他们的值是一个整数，按照元素声明时候的顺序从0开始依次进行+1操作，默认的值就是：0,1,2,3，...
   例如，上面的ESex枚举类型中，ESex_Male 的值默认是0，ESex_FMale 的默认值是1，依此类推。

3. 枚举元素有默认的值，但也可以在声明的时候指定值，例如：
   enum EWeekDay
   {
       EWeekDay_1 = 3,
       EWeekDay_2 = 4,
       EWeekDay_3 = 5,
       EWeekDay_4,
       EWeekDay_5,
       EWeekDay_6,
       EWeekDay_7,
   };
   其中从 EWeekDay_4 开始未赋值，所以按照他的上一个元素的值+1的规则进行默认赋值，也就是 EWeekDay_3 + 1 = 6。
   注意事项：上面赋值的最好是依次增大，不然有可能会造成两个枚举元素是一样的值，例如：
   enum EWeekDay
   {
       EWeekDay_1 = 3,
       EWeekDay_2 = 2,
       EWeekDay_3 = 1,
       EWeekDay_4,
       EWeekDay_5,
       EWeekDay_6,
       EWeekDay_7,
   };

4. 枚举值可以用来进行跟整数一样的判断，比较，switch-case 等操作（也就是当成一个整型常量来使用）。

   例如：
   int ab = 2;
   if(EWeekDay_1 > ab )
   {
     //...
   }

   再比如比较常用的和switch-case一起用：

   EWeekDay day;

   switch (day) //小助手会自动帮你生成大括号里的内容

   {

   case EWeekDay_1:

   ​    break;

   case EWeekDay_2:

   ​    break;

   case EWeekDay_3:

   ​    break;

   //...

   }

5. 虽然枚举类型的变量可以看做是整形类型的变量，但是不能把普通的整数赋值给枚举类型变量，例如：

   EWeekDay day = 2;
   这种是错误的，除非进行强制类型转换：

   EWeekDay day = (EWeekDay) 2;

   但是不建议。最好还是：
   EWeekDay day = EWeekDay_2;

   这样也可以但是没有意义：

   EWeekDay day = (EWeekDay) 20;

## 和结构体类型结合

```c++
//eg.
enum ESex
{
    ESex_Male, //男性
    ESex_FMale //女性
};

struct Student
{
    string name;
    int num;
    ESex sex;
    int age;
};

int main()
{
    Student stu;
    stu.sex = ESex_Male;
}
```

# typedef

除了可以用 struct 结构体，union 联合体，enum 枚举 等自定义类型以外，还可以使用 typedef 声明一个新的类型名字来代替已有的类型名。注意是新的类型名字，只是名字而已，不是一种全新的类型，只是改个名字而已。
例如，我们定义一个无符号的整型int变量可以这样来定义：unsigned int a = 5;
类型的名字比较长，unsigned int，而且以后所有定义无符号的整型int变量都得这么写，那么有没有简单的写法呢，typedef就派上用场了，咱们可以给 unsigned int 改个名字，例如：
typedef unsigned int uint;
uint a = 5;
所以，以后所有的 unsigned int 都可以改成 uint 了，同理，其他的类型也都可以使用 typedef 改名，例如：
typedef int myint;
typedef unsigned long ulong;
typedef Student StuT;
typedef EWeekDay EWDay;
以上，都是可以的哦！ 

# VS编程实用技巧

## C++保留关键字

不能用这些关键字来作为变量的名字等用途，否则编译报错。
详见：https://www.cctry.com/thread-289434-1-1.html 

## 注释

右上方工具栏有用来注释和取消注释的两个按钮。

单行注释：将光标定位到所在的行，选中不选中都可以。之后点击工具栏的注释图标即可；
多行注释：鼠标选中所要注释的行，之后点击工具栏的注释图标即可。 

## 一闪而过的黑窗口

程序正常执行完了之后就退出，是正常现象。VS2019的黑窗口默认不会一闪而过。

很多C++小白用户前期第一次使用VC6.0或者VS2013进行调试的时候，一般都是按F5，之后程序启动起来，黑窗口一闪而过，连程序执行的结果什么的都没看到，类似这样的情况如何操作呢？

1. getchar/cin 法：在结尾加一句getchar()/cin >> a; 按任何字母数字等东西后退出
2. 断点法：设个断点
3. Ctrl+F5 法（开始执行执行（不调试），编译器帮我们停住）
4. 工具栏添加快捷按钮点击法：菜单栏最右边的▼（工具栏选项）-> 添加或移除按钮 -> 自定义 -> 添加命令 -> 调试选项中找到第3点的开始执行（不调试）-> 点击确定，添加到菜单栏 -> 通过上移下移删除等按钮布置其在菜单栏的位置。

## 精简工程源码

1. 工程目录下的Debug/Release文件夹都可以删除，包括两层目录结构（Debug/Release文件各两个，总共4个文件）

2. 工程目录下的：SolutionName.sdf、ipch文件夹 以及 *.pch 等文件都可以直接删除，大大减少体积；
   备注：VC6.0的话有个 *.ncb 文件也可以删除。

   如果不想让VS在工程目录中生成这些的文件，可以按照如下设置：
   工具(T)->选项(O)...->文本编辑器->C/C++->高级->正在浏览数据库回退（VS2019）/回退位置（VS2013）：
   始终使用回退位置->True
   回退位置已在使用时，不警告->True
   回退位置->留空

   经过以上设置之后，VS会把 sdf、pch 等相关文件生成在系统的临时目录中，从而不生成在工程目录中，所以以后打包的时候就方便了，不用每次都清理。 

## 一些操作

运行程序：Ctrl + F5 或调试 >“开始执行(不调试)”菜单
调试程序：F5 或调试 >“开始调试”菜单
停止调试：shift + F5
设置断点：将光标定位到所在的行，F9
调试时，选中变量拖至监视窗口，可查看详细信息；变量前加&可以看地址在哪