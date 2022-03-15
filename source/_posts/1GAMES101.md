---
title: 从1开始的图形学入门生活（Transformation & 作业0）
date: 2022-2-27 14:58:00
tags: 
 - 图形学
 - 入门
categories: 
 - 图形学
cover: /img/19.jpg
---

{% note info %}
课程链接：https://www.bilibili.com/video/BV1X7411F744?from=search&seid=944040088923830300&spm_id_from=333.337.0.0

课程PPT：https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html

作业链接：http://games-cn.org/forums/topic/allhw/

{% endnote %}

{% note warning %}
今天发现贴代码&lt &gt 消失的原因是，html里面大小括号要用转义字符 = =

但是转义字符连着include的写还是会消失(ˉ  ˉ；)...

{% endnote %}

# Viewing transformation

## Projection transformation

### Orthographic projection

无近大远小

相机离的无限远

#### 具体操作

- Camera located at origin, looking at -Z, up at Y (looks familiar?) 
- Center cuboid by translating   
- Scale into “canonical” cube       **方便之后的计算*

#### Transformation matrix

![](1645864740675.png)

#### Caveat

- 因为我们是从-Z方向看的，所以近的z大，远的z小
- FYI: that’s why OpenGL (a Graphics API) uses left hand coords.  

### Perspective projection

有近大远小

#### 具体操作

- First “squish” the frustum into a cuboid (n -> n, f -> f) (Mpersp->ortho) 
- Do orthographic projection (Mortho, already known!) 

![](1645866490259.png)

#### Transformation matrix

由相似三角形出发，加上特殊点的坐标推导，可得步骤一的矩阵为：

![](1645866761408.png)

![](1645866801584.png)

# 作业0

## Eigen的简单使用

点乘：dot()

叉乘：cross()

头文件声明：

```c++
#include &lt eigen3/Eigen/Core &gt
```

矩阵声明：

```c++
//n阶方阵
Eigen::Matrix2d m_matrix;
m_matrix<< 2,3,2.2,1;
std::cout << "m_matrix = \n"<< m_matrix<std::endl;
//mxn方阵
Eigen::MatrixXd xx(5,2);
xx << 2,3,2.2,1,2,3,2,1,2,3;
//零矩阵
Eigen::MatrixXf m_matrix = Eigen::MatrixXf::Zero(5,5);
//对角矩阵
Eigen::MatrixXf m_matrix = Eigen::MatrixXf::Identity(5,5);
```

> 更多基础操作见：https://blog.csdn.net/weixin_42156097/article/details/108011364?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-2-108011364.pc_agg_new_rank&utm_term=eigen%E5%BA%93%E4%B8%AD%E6%80%8E%E4%B9%88%E5%AE%9A%E4%B9%89%E7%9F%A9%E9%98%B5&spm=1000.2123.3001.4430
>
> Eigen库头文件类型：
>
> https://www.jianshu.com/p/931dff3b1b21

## 作业0

### 作业描述

给定一个点P=(2,1), 将该点绕原点先逆时针旋转45◦，再平移(1,2), 计算出
变换后点的坐标（要求用齐次坐标进行计算）。

### 解答

```c++
#include &lt cmath &gt 
#include &lt eigen3/Eigen/Core &gt
#include &lt eigen3/Eigen/Dense &gt
#include &lt iostream &gt
using namespace std;

int main(){
    Eigen::Vector3f P(2,1,1);
    Eigen::Matrix3f TR;
    //TR << 0.5, -0.5, 1, 0.5, 0.5, 2, 0, 0, 1;
    float a = 45.0/180.0*M_PI;
    TR << cos(a), -sin(a), 1, 
          sin(a),  cos(a), 2, 
             0,      0,    1;
    Eigen::Vector3f P1;
    P1 = TR * P;
    cout << "After transformation, P = (" << P1[0] << ","<< P1[1]<<")" << endl;

    return 0;
}

```

注意在C++中，cos、sin函数的参数为弧度制。

P = （1.70711， 4.12132）

### 编译

```shell
mkdir build
cd build
cmake ..
make
./Transformation
```

