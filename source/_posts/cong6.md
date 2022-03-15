---
title: 从6开始的C++世界生活（结构体）
date: 2020-06-19 23:02:15
tags:  
 - C++
 - 入门
categories: 
 - C++
cover: /img/15.jpg

---

{% note info %}
本篇是学习VC驿站的《实用C++》教程时的超简化同步笔记，详细内容可参见VC驿站原帖
{% endnote %}

{% note warning %}
是在学校C++课程之外重新系统地学习一遍C++，所以很基础很基础的部分不会记录

代码块渲染有误，目前不知道怎么解决，#include后面的总是消失，所以#include统一写作包含，懂什么意思就行了

如果觉得网站字体太小，可以在右下方的按钮中点击+放大哦√（比如我就觉得这个网站的字体很小）

{% endnote %}

{% note success %}
希望对你和我都能有所帮助 : ）
{% endnote %}

# 概述

**<u>自定义</u>数据类型：**

C/C++语言本身提供了很多基本数据类型，例如：int、float、char 等供我们使用。但是程序编写的过程中问题往往比较复杂，基本的数据类型有时候不能满足我们的需求，所以C/C++语言允许开发者根据自己的需要自定义数据类型，接下来要讲解的结构体struct、联合体union、枚举类型enum，类类型class 等就是用户自定义的数据类型。这些用户自定义的数据类型跟C/C++语言提供的基本类型一样，都可以用来定义变量。只不过在这些自定义数据类型在使用之前要先由用户声明出来才行。 

# 基本用法

## 声明

 struct 结构体类型名
{
  //成员表;
};

struct 是声明该类型为结构体类型的关键字，不能省略。

结构体类型名就是该结构体类型的名字，以后可以直接拿这个类型名来定义变量，就跟使用int，double一样用。

类型名的命名规则跟变量一样，可以是数字、字母、下划线，且数字不能开头。习惯上，类型名的首字母大写。

备注：C语言中结构体的成员只能是数据，C++对此进行了扩充，结构体的成员既可以包含数据，也可以包含函数，其实在C++中 struct 跟 class 从使用角度来说差别不大。

## 定义

1. 结构体类型声明完了之后就可以定义变量了，如下：
   Student zhangsan, lisi;
   这种是非常常用的一种定义结构体类型变量的方法。

2. 当然也可以在声明结构体类型的时候就定义变量，当然这种是不常用的方法：（比较老的写法，不是很方便，更建议是哪里要用到时再定义，也就是第一种定义方式）
   struct Student
   {
     string name;
     int num;
     char sex;
     int age;
   } zhangsan, lisi;

## 初始化

 Student zhangsan = {"张三", 1001, 'm', 25};
备注：

1. 初始化参数的顺序一定要和结构体类型声明的成员表顺序一致才行，不然会报错而且会错误的赋值。 
2. 如果初始化的值没给全，后面默认是0。比如Student zhangsan = {"张三", 1001, 'm'}; ，那么最后一个成员age的值就是0。

## 成员的访问

结构体变量名.成员名
可以用这种方式来访问。
例如：
Student zhangsan = {"张三", 1001, 'm', 25};
zhangsan.num = 29;
int num = zhangsan.num; 

# 结构体数组与指针

## 结构体类型+数组

```c++
struct Student
{
    string name;
    int num;
    char sex;
    int age;
};

Student stu[100];
```

## 结构体类型变量作为函数参数

```c++
struct Student
{
        string name;
        int num;
        char sex;
        int age;
};

void set_num(Student stu)
{
        static int stu_num = 101;
        stu.num = stu_num++;
}

int main(int argc, char* argv[])
{
        Student stu[2] = 
        {
                { "aaa", 0, 'f', 10 },
                { "bbb", 0, 'm', 12 }
        };

        //遍历
        for (int idx = 0; idx < 2; ++idx)
        {
                set_num(stu[idx]);
                cout << stu[idx].name << " " << stu[idx].num << " " << stu[idx].sex << " " << stu[idx].age << endl;
        }

        return 0;
}
```

 以上代码中的 set_num 函数真的能实现修改 stu 的学号吗？答案是否定的。

结构体类型变量作为函数的参数传递的时候也是以**值传递**的方式进行的，在函数调用的过程中实参 stu[idx] 会**拷贝**一份给形参，即进行 Student 类型变量的拷贝工作，会将 Student 类型内部的变量一个一个的复制过去。所以 set_num 函数是没办法修改外部的 stu[2] 数组里面的学生学号信息的。

解决方法：将参数改成传递 Student* 指针的方式。

一方面可以保证 set_num 函数可以**修改外部的实参**；还有一个好处就是，可以防止 Student 类型变量的拷贝工作，**提高效率**。当前 Student 类型变量内部只有四个成员，但是在以后的编程过程中某个结构体或者类对象中可能包含几十个成员，这个时候如果进行拷贝的话，开销会很大。所以有时候，即使不需要在函数的内部修改实参的值，函数的参数类型也定义成指针的类型，就是这个目的。 

## 用指针访问成员变量

```c++
Student stu;
Student* pstu = &stu;
(*pstu).num = 102;
```

写起来比较麻烦，所以规定结构体类型的指针变量可以用 -> 符号来引用其成员

```c++
Student stu;
Student* pstu = &stu;
pstu->num = 102;
```



所以对set_num函数进行如下改写：

```c++
struct Student
{
        string name;
        int num;
        char sex;
        int age;
};

void set_num(Student* pstu)
{
        static int stu_num = 101;
        pstu->num = stu_num++;
}

int main(int argc, char* argv[])
{
        Student stu[2] = 
        {
                { "aaa", 0, 'f', 10 },
                { "bbb", 0, 'm', 12 }
        };

        //遍历
        for (int idx = 0; idx < 2; ++idx)
        {
                set_num(&stu[idx]);
                cout << stu[idx].name << " " << stu[idx].num << " " << stu[idx].sex << " " << stu[idx].age << endl;
        }

        return 0;
}
```

# 小作业

## 小作业1（关于左值++）

根据运算符的优先级，大家想一下：
Student stu[2] =
{
    { "aaa", 0, 'f', 10 },
    { "bbb", 0, 'm', 12 }
};

Student* p = stu;
++p->num = 202; //这句代码执行之后，到底是stu[0] 的 num 值变化了，还是 stu[1] 的 num 值变化了？ 

------

实验了一下这段代码，答案是stu[0]的num变成了202。

查表知，->优先级大于++，所以那句代码就相当于++(p->num) = 202。

然后又在想，++(p ->num)是什么意思，202赋值给++变量？根据这个思考去查了查，得到如下结果：

> 首先说左值和右值的定义：
> 变量和文字常量都有存储区，并且有相关的类型。区别在于变量是可寻址的（addressable），对于每一个变量都有两个值与其相联：
> 1).它的数据值，存储在某个内存地址中。有时这个值也被称为对象的右值（rvalue,读做are-value）。我们也可认为右值的意思是被读取的值（read value）。文字常量和变量都可被用作右值。
> 2).它的地址值——即存储数据值的那块内存的地址。它有时被称为变量的左值（lvalue，读作ell-value）。我们也可认为左值的意思是位置值（location value）。文字常量不能被用作左值.
>
> 首先对于i++的实现是：
> int temp;
> temp = i;
> i = i+1;
> return temp;
> 而++i的实现是：
> i = i+1;
> return i;
> 所以对于我们提出来的问题已经能得到解决了：
> i++=5; 是错误的。因为i++返回的是编译器自动分配的临时变量temp，而这个temp并不是你程序中定义的可寻址变量的引用 ，也就是说你不能通过地址对temp进行操作(换句话说就是不能作为左值)，因为temp是一个临时变量。
> ++i=5; 是正确的。因为其返回值就是i。

虽然对于这个东西的本质还是有点懵懵懂懂的，但是理解一下，++(p->num) = 202 应该是对p->num变量加1后返回了p->num变量（也就是左边变成了值为1的p->num变量），再对p->num变量赋值为202，所以结果就是stu[0]的num变成了202。

## 小作业2（比较简单）

定义一个学生类型的结构体，包含学生的：姓名、学号、分数。之后用该结构体定义大小为5的结构体变量数组。手动输入给数组成员赋值，之后将5个学生的信息输出出来，并且求出5个学生的分数的平均值也一起输出出来。

```c++
struct Student
{
	string name;
	int num;
	float score;
};

int main()
{
	float sum = 0;
	Student stu[5];
	for (int i = 0; i < 5; i++)
	{
		cout << "请输入第" << i + 1 << "个学生的信息" << endl;
		cout << "姓名："; cin >> stu[i].name;
		cout << "学号："; cin >> stu[i].num;
		cout << "分数："; cin >> stu[i].score;
		sum += stu[i].score;
	}
	cout << endl << "该5名学生的信息如下所示：" << endl;
	for (int i = 0; i < 5; i++)
	{
		cout << "第" << i + 1 << "个的学生信息为：" << endl;
		cout << "姓名：" << stu[i].name << endl;
		cout << "学号：" << stu[i].num << endl;
		cout << "分数：" << stu[i].score << endl;
	}
	cout << "平均分为：" << sum / 5 << endl;
}
```



