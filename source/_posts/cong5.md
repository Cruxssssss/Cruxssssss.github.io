---
title: 从5开始的C++世界生活（指针、数组指针）
date: 2020-06-19 16:32:05
tags:  
 - C++
 - 入门
categories: 
 - C++
cover: /img/13.jpg

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

# 指针

## 一些细节

1. char ch = 'a';
   cout << ch;

   使用cout输出时，cout会去找ch的地址，判断ch的类型（char类型，所以从ch的地址开始读取一个字节），然后进行打印输出。

2. 指针的英文是pointer，所以可以看到定义指针时它的名字一般是p。

3. 对一个变量的修改，既可以通过该变量自身（直接修改），也可以通过指针/地址来实现修改（间接修改）。 

## 指针变量的定义及初始化

1. 符号* 既可以靠近基类型，也可以靠近指针变量名

   例如：int* p; 和 int *p; 都是正确的。

2. 指针变量可以在定义的时候就初始化，也可以先定义后初始化，也可以在以后的任意一个时间去指向某个变量的地址：
   int a = 5;
   int *pa;
   pa = &a;
   int *pb = &a; 

3. 定义一个函数，实现交换两个参数的值

   ```c++
   void swap(int* pa, int* pb)
   {
       int t = *pa;
       *a = *b;
       *b = t;
   }
   ```

# 数组与指针

1. C/C++中规定数组名字就是数组的首地址。注意，是数组的首地址，也就是数组的第0个元素的地址，即：
   int num[100] = {0};
   int* pnum = &num[0];
   这里的 num 和 pnum 的值应该是一样的 
2. C/C++中规定如果指针变量p已经指向数组中的一个元素，则p+1指向同一数组中的下一个元素的地址（而不是将p的值简单+1），p-1同理。
   int num[100] = {0};
   int* pnum_0 = &num[0];
   int* pnum_1 = &num[1];
   pnum_0 += 1;
   执行以上代码之后，这里面pnum_0的值应该和pnum_1的值是相等的。 
3. 数组的名字确实是数组的首地址，可以把他理解为一个指针但是这个指针是：int * const 类型的，也就是说不能改变其指向的地址。也就是说不能动是固定的。那怎么办呢？
   没关系，我们可以再定义一个指针，如下：
   int num[100] = {0};
   int* pnum = num;
   这里的 pnum 是可以移动的，所以就可以指向数组num中的任意一个元素了。例如遍历一个数组：
   int num[10] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
   int* pnum = num;
   for(int idx = 0; idx < 10; ++idx)
   {
     cout << *(pnum+idx) << endl;
   }
4. 访问数组的两种方法：
   下标法：num[0];
   指针法：*(pnum+idx) 

# 小作业

## 小作业1

```c++
//实现一个函数，作用是获取一个数组中的最大值和最小值，函数声明如下：
void get_min_max(int src[], int* max_v, int* min_v)
{
	*max_v = *min_v = src[0];
	for (int i = 0; i < 10; i++)
	{
		*max_v = (*max_v > src[i]) ? *max_v : src[i];
		*min_v = (*min_v < src[i]) ? *min_v : src[i];
	}
}
```

问题是怎么得到整型数组的大小。

查找得：

------

c语言中，定义数组后可以用sizeof命令获得数组的长度（可容纳元素个数）。
例如：

```c++
int data[4];
int length;
length=sizeof(data)/sizeof(data[0]);  //数组占内存总空间，除以单个元素占内存空间大小
printf("length of data[4]=%d", length ); //输出length of data[4]=4
```

但是，通过传递数组名参数到子函数中，以获得数组长度是不可行的。

例如：

```c++
int getLength(int a[]){
    int length;
    length=sizeof(a)/sizeof(a[0]); //这样是错误的，得到的结果永远是1
    return length;
}
```

因为，a是函数参数，到了本函数中，a只是一个指针（地址，系统在本函数运行时，是不知道a所表示的地址有多大的数据存储空间，这里只是告诉函数：一个数据存储空间首地址），所以，sizoef(a)的结果是指针变量a占内存的大小，一般在32位机上是4个字节。a[0]是int类型，sizeof(a[0])也是4个字节，所以，结果永远是1。
因此，获得数组长度，只能在数组定义所在的代码区中，采用以上方法，才可以达到效果。

------

然后在VS上试了试第二种方式算长度，输出的确是1。

所以这道题应该是得在主函数内用第一种方法算出数组长度，再把长度传参给函数吧。

## 小作业2

 定义两个字符数组输入如下：
char str1[50] = {0}, str2[] = "I Love cctry.com!";
用指针的方式，实现将 str2 拷贝到 str1 中。 

```c++
char str1[50] = {0}, str2[] = "I Love cctry.com!";
char *pstr1 = str1, *pstr2 = str2;
for(; *pstr2 != 0;)
{
    *(pstr1++) = *(pstr2++);  //注意！！这里str2中的0没有复制过去！
}
cout << "str1 = " << str1 << endl;
```

## 小作业3

输入一个字符串，例如：
a123x456__17960?302ab5876
将其中连续的数字作为一个整数，依次存放到一个数组中a中，例如：123放在a[0]中，456放在a[1]中。统计共有多少个整数，并输出这些整数。

需要包含的文件：

iostream
math.h

```c++
using namespace std;

int main()
{
	char in[100] = "a123x456__17960 ? 302ab5876";
	int flag1 = 0, flag2 = 0, count = 0, len, start, end, out[100], j=0, num, total;
	for (int i = 0; i < strlen(in); i++)//这里报warning C4018: “<”: 有符号/无符号不匹配，不太懂
	{
		if (in[0] >= '0' && in[0] <= '9')
		{
			flag1 = 1;
			start = 0;
		}
		if ((in[i] < '0' || in[i] > '9') && (in[i + 1] >= '0' && in[i + 1] <= '9'))
		{
			flag1 = 1;
			start = i + 1;
		}
		if ((in[i + 1] < '0' || in[i + 1] > '9') && (in[i] >= '0' && in[i] <= '9'))
		{
			flag2 = 1;
			end = i;
		}
		if (in[strlen(in)] >= '0' && in[strlen(in)] <= '9')
		{
			flag2 = 1;
			end = strlen(in);
		}
		if (flag1 == 1 && flag2 == 1)
		{
			total = 0;
			flag1 = flag2 = 0;
			count++;
			len = end - start;
			for (int k = start; k <= end; k++)
			{
				num = in[k] - '0';
				total += num * pow(10.0, len);
				len--;
			}
			out[j] = total;
			j++;
		}
	}
	cout << "共有" << count << "个整数" << endl;
	for (int i = 0; i < count; i++)
	{
		cout << out[i] << " ";
	}
}
```



 