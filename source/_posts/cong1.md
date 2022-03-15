---
title: 从1开始的C++世界生活
date: 2020-05-26 23:44:40
tags: 
 - C++
 - 入门
categories: 
 - C++
cover: /img/8.jpg
---

{% note info %}
本篇重点：输入输出语句
{% endnote %}

# 语句

1. 空语句;;;;;;;
2. 无意义语句 x + y;（编译器会报warning）
3. 复合语句：大括号{}包括起来的多条语句

# 输入输出语句

C++兼容C语言的输入与输出



------

初学者下面的知识点不用深究！！！

> ​        可以这么说，本身大家以后编写控制台类型的程序就不多，另外，对于输入和输出这块也不用掌握的非常深入，所以C语言的 getchar、putchar、scanf、printf 之类的大家有时间就学学，没时间就先放弃也行。把 cin 和 cout 的一些基本的使用弄明白了就可以了。以后真要是有用到的地方再去现查就来得及。不用一开始就细致入微，本身刚开始接触编程有很多东西就不是很懂所以不要求深扣某个细节，可以先大体上把知识点过一遍，之后从头再过一遍，每一遍都比前一遍要细致一些要深入一些，每一遍都可以学到不同的东西。

------



## cin详细用法 

具体参见：https://www.cctry.com/thread-279040-1-1.html 

------



### 缓冲区：  

​        当我们从键盘输入字符串的时候需要敲一下回车键才能够将这个字符串送入到缓冲区中，那么敲入的这个<u>回车键(\r)会被转换为一个换行符\n，这个换行符\n也会被存储在cin的缓冲区中并且被当成一个字符来计算</u>！比如我们在键盘上敲下了123456这个字符串，然后敲一下回车键（\r）将这个字符串送入了缓冲区中，那么此时缓冲区中的字节个数是7 ，而不是6。

​        cin读取数据也是从缓冲区中获取数据，缓冲区为空时，cin的成员函数会阻塞等待数据的到来，一旦缓冲区中有数据，就触发cin的成员函数去读取数据。  



------



### cin >>

​        cin可以连续从键盘读取想要的数据，以<u>空格、tab或换行</u>作为分隔符。

**注意：**
（1）cin>>等价于cin.operator>>()，即调用成员函数operator>>()进行读取数据。
（2）当cin>>从缓冲区中读取数据时，若缓冲区中第一个字符是空格、tab或换行这些分隔符时，cin>>会将其<u>忽略并清除</u>，继续读取下一个字符，若缓冲区为空，则继续等待。但是如果读取成功，<u>字符后面的分隔符是残留在缓冲区</u>的，cin>>不做处理。
（3）不想略过空白字符，使用 noskipws 流控制。比如cin>>noskipws>>input;



------



### cin.get

**概述：**

​        该函数有多种重载形式，分为四种格式：无参，一参数，二参数，三个参数。常用的的函数原型如下：

```c++
int cin.get();
istream& cin.get(char& var);
istream& get ( char* s, streamsize n );
istream& get ( char* s,  streamsize  n, char delim )
```

其中streamsize 在VC++中被定义为long long型。

另外，还有两个重载形式不怎么使用，就不详述了，函数原型如下：

```c++
istream& get ( streambuf& sb);
istream& get ( streambuf& sb, char delim );
```



#### 读取一个字符

​        可以使用cin.get或者cin.get(var)

eg.

a=cin.get();

cin.get(b);

**注意：**
（1）cin.get()从输入缓冲区读取单个字符时不忽略分隔符，直接将其读取。
（2）cin.get()的返回值是int类型，成功：读取字符的ASCII码值，遇到文件结束符时，返回EOF，即-1，Windows下标准输入输入文件结束符为Ctrl+z，Linux为Ctrl+d。

​        cin.get(char var)如果成功返回的是cin对象，因此可以支持链式操作，如cin.get(b).get(c)。*（链式操作？）*



#### 读取一行

​        可以使用 istream& get ( char* s, streamsize n ) 或 istream& get ( char* s, size_t n, streamsize delim )。二者的区别是前者默认以换行符结束，后者可指定结束符。n表示目标空间的大小。

 **注意：**
（1）从结果可以看出，cin.get(array,20);读取一行时，遇到换行符时结束读取，但是不对换行符进行处理，换行符仍然残留在输入缓冲区。

​        getline读取一行字符时，默认遇到’\n’时终止，并且将’\n’直接从输入缓冲区中删除掉，不会影响下面的输入处理。

（2）cin.get(str,size);读取一行时，只能将字符串读入C风格的字符串中，即char*，但是C++的getline函数可以将字符串读入C++风格的字符串中，即string类型。

​        鉴于getline较cin.get()的这两种优点，建议使用getline进行行的读取。  

 

------



### cin.getline

​        从标准输入设备键盘读取一串字符串，并以指定的结束符结束。

函数原型：*（和cin.get读行的用法几乎一样）*

```c++
istream& getline(char* s, streamsize count); //默认以换行符结束
istream& getline(char* s, streamsize count, char delim);
```

与cin.get的区别：cin.getline不会将结束符或者换行符残留在输入缓冲区中；

​                               可以将字符串读入C++风格的字符串中，即string类型。

 

### cin的条件状态

具体有哪些条件状态参考已给链接。把样例贴上来，因为经常遇到，有助于理解：

```c++
#include <iostream>
using namespace std;

int main()
{
char ch, str[20]; 
    cin.getline(str, 5);
    cout<<"flag1:"<<cin.good()<<endl;    // 查看goodbit状态，即是否有异常
    cin.clear();                         // 清除错误标志
    cout<<"flag1:"<<cin.good()<<endl;    // 清除标志后再查看异常状态
    cin>>ch; 
    cout<<"str:"<<str<<endl;
    cout<<"ch :"<<ch<<endl;

    system("pause");
    return 0;
}
```

输入：12345[回车]

输出结果为：

> 12345
>
> flag1:0
>
> flag1:1
>
> str:1234
>
> ch:5

分析：

​        因输入缓冲区未读取完造成输入异常，通过clear()可以清除输入流对象cin的异常状态，不影响后面的cin>>ch从输入缓冲区读取数据。

​        如果将clear()注释，cin>>ch; 将读取失败，ch为空。

cin.clear()等同于cin.clear(ios::goodbit); 因为cin.clear()的默认参数是ios::goodbit，所以不需显示传递，故而你最常看到的就是：cin.clear();  

 

------



### cin 清空输入缓冲区

​        从上文中可以看出，上一次的输入操作很有可能是输入缓冲区中残留数据，影响下一次的输入。那么如何解决这个问题呢？自然而然，我们想到了在进行输入时，对输入缓冲区进行清空和状态条件的复位。条件状态的复位使用clear()，清空输入缓冲区应该使用：

函数原型：

​        istream &ignore( streamsize num=1, int delim=EOF );

函数作用：

​        跳过输入流中n个字符，或在遇到指定的终止字符时提前结束（此时跳过包括终止字符在内的若干字符）。

```c++
cin.ignore(numeric_limits<std::streamsize>::max(),'\n'); //清除缓冲区的当前行
cin.ignore(numeric_limits<std::streamsize>::max()); //清除cin里所有内容
```

  **注意：**
（1）程序中使用cin.ignore清空了输入缓冲区的当前行，使上次的输入残留下的数据没有影响到下一次的输入，这就是ignore()函数的主要作用。

​        其中，numeric_limits::max()是头文件定义的流使用的最大值，你也可以用一个足够大的整数代替它。

（2）cin.ignore()；当输入缓冲区没有数据时，也会阻塞等待数据的到来。  

（3）有个疑问，网上很多资料说调用cin.sync()即可清空输入缓冲区，本人*（不是我，是Syc）*测试了一下，VC++可以，但是在linux下使用GNU C++却不行，无奈之下，linux下就选择了cin.ignore()。





### 其它从标准输入读取一行字符串的方法

#### getline读取一行

​        C++中定义了一个在std名字空间的全局函数getline，因为这个getline函数的参数使用了string字符串，所以声明在了< string>头文件中了。
​        getline利用cin可以从标准输入设备键盘读取一行，当遇到如下三种情况会结束读操作：

​        1）到文件结束

​        2）遇到函数的定界符

​        3）输入达到最大限度



函数原型有两个重载形式：  

```c++
istream& getline ( istream& is, string& str);//默认以换行符结束
istream& getline ( istream& is, string& str, char delim);

//eg
    string str;
    getline(cin,str);
```

**注意：**

​        getline遇到结束符时，会将结束符一并读入指定的string中，再将结束符替换为空字符。因此，进行从键盘读取一行字符时，建议使用getline，较为安全。但是，最好还是要进行标准输入的安全检查，提高程序容错能力。
​        cin.getline()类似，但是cin.getline()属于istream流，而getline()属于string流，是不一样的两个函数。

 

#### gets读取一行

​        gets是C中的库函数，在< stdio.h>申明，从标准输入设备读字符串，可以无限读取，不会判断上限，以回车结束或者EOF时停止读取，所以程序员应该确保buffer的空间足够大，以便在执行读操作时不发生溢出。

函数原型：char *gets( char *buffer );

```c++
//eg
    char array[20]={NULL};
    gets(array);
```

​        该函数是C的库函数，所以不建议使用

------



## 标准输入输出流控制符

![](1.jpg)

详见：https://www.cctry.com/thread-279037-1-1.html 

虽然此处不必深究细节，但还是拎出来几个有意思的小点：~~（虽然有些地方不懂）~~

- setprecision(n)  设置实数的精度为n位。在以一般十进制小数形式输出时，n代表有效数字。在以fixed(固定小数位数)形式和scientific(指数)形式输出时，n为小数位数。
- 成员函数width(n)和控制符setw(n)只对其后的第一个输出项有效。如果要求在输出数据时都按指定的同一域宽n输出，不能只调用一次width(n)，而必须在输出每一项前都调用一次width(n)。
- 输出格式状态分为5组，每一组中同时只能选用一种(例如，dec，hex和oct中只能选一，它们是互相排斥的)，在用成员函数serf和控制符setiosflags设置输出格式状态后，如果想改设置为同组的另一状态，应当调用成员函数unsetf(对应于成员函数serf)或resetiosflags(对应于控制符sefiosflags)，先终止原来设置的状态。然后再设置其他状态。
- 用serf函数设置格式状态时，可以包含两个或多个格式标志，由于这些格式标志在lOS类中被定义为枚举值，每一个格式标志以一个二进位代表，因此可以用“位或”运算符“I”组合多个格式标志。*（此处懵逼）*
- 对输出格式的控制，既可以用控制符，也可以用cout流的有关成员函数，二者的作用是相同的*（不知道 cout << fixed << setprecision(2)中的 fixed 是用的什么方式设置的，第一种吗？）*。控制符是在头文件mmamp*（sic）*中定义的，因此用控制符时，必须包含iomanip头文件。cout流的成员函数是在头文件iostream中定义的，因此只需包含头文件iostream，不必包含iomanip。许多程序人员感到使用控制符方便简单，可以在一个cout输出语句中连续使用多种控制符。
- 使用中还会遇到一些细节问题，不可能在这里全部涉及。在遇到问题时，请查阅专门手册或上机试验一下即可解决。



------

呜呜呜重学一遍没想到这一节居然有些难啃qwqqqqqqqqqqqq

本来甚至打算把所有基础语句都放在一节的呜呜呜，结果光是输入输出就已经！

我不行了我累死了要哒宰亲亲抱抱举高高才能继续学下去（bushi

![](2.jpg)



# 最后细数这几天写博客遇到的坑

1. 不要用制表符！代码只用空格就好，然后正文部分用无序列表
2. #include< iostream>总是显示不出来淦！
3. 千万不要用五级标题，比正文的字还小= =
4. < >加空格防止markdown把它变成链接啥的
5. 有时候你以为换行了，只是你以为的换行，实际上没有换行
6. 换行分割线，分割线上下距离相同