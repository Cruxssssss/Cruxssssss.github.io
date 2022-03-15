---
title: 从2开始的图形学入门生活（Rasterization(Triangles) & 作业1）
date: 2022-2-28 10:40:00
tags: 
 - 图形学
 - 入门
categories: 
 - 图形学
cover: /img/20.jpg
---

{% note info %}
课程链接：https://www.bilibili.com/video/BV1X7411F744?from=search&seid=944040088923830300&spm_id_from=333.337.0.0

课程PPT：https://sites.cs.ucsb.edu/~lingqi/teaching/games101.html

作业链接：http://games-cn.org/forums/topic/allhw/

{% endnote %}

# Rasterization(Triangles)

## 定义Frustum

field-of-view：垂直可视角度（角度fovY）

aspect ratio：高宽比（宽 : 高）

## Canonical Cube to Screen 

 ### What is a screen?

- An array of pixels
- Size of the array: resolution
- A typical kind of raster display （光栅化设备）

Raster == screen in German

- Rasterize == drawing onto the screen

Pixel (FYI, short for “picture element”) 

- For now: A pixel is a little square with uniform color
- Color is a mixture of (red, green, blue) 

### Defining the screen space  

![](1645951047871.png)

![](1645951153351.png)

## Rasterization: Drawing to Raster Displays

 ###  Triangles - Fundamental Shape Primitives 

Why triangles?

- Most basic polygon
  * Break up other polygons  

- Unique properties
  * Guaranteed to be planar 
  * Well-defined interior 
  * Well-defined method for interpolating values at vertices over triangle (barycentric interpolation)

### Approch:  Sampling A 2D Indicator Function 

用像素中心对屏幕空间采样

```c++
for (int x = 0; x < xmax; ++x)
	for (int y = 0; y < ymax; ++y)
 		image[x][y] = inside(tri,x + 0.5,y + 0.5); 
```

#### 包围盒

![](1645960486064.png)

###  On Real Displays 

三星屏幕绿色更多，因为人眼对绿色更敏感

### Problems

- Jaggies（锯齿）
- Aliasing（走样）

光栅化图形学致力于解决的问题：如何抗锯齿、反走样

# 作业1

## Questions

- get_model_matrix(float rotation_angle): 逐个元素地构建模型变换矩
  阵并返回该矩阵。在此函数中，你只需要实现三维中绕z 轴旋转的变换矩阵，
  而不用处理平移与缩放。
- get_projection_matrix(float eye_fov, float aspect_ratio, float
  zNear, float zFar): 使用给定的参数逐个元素地构建透视投影矩阵并返回
  该矩阵。
- [提高项5 分] 在main.cpp 中构造一个函数，该函数的作用是得到绕任意
  过原点的轴的旋转变换矩阵。
  Eigen::Matrix4f get_rotation(Vector3f axis, float angle)

## Answer

### Tips

1. 角度转换为弧度制

2. 参数zNear，zFar是正数，n、f 是坐标（负数）

3. `abs()` 求绝对值

4. `transpose()` 矩阵转置

   但如果想要transpose可以赋值给自身，用`transposeInPlace()`

5. 矩阵提取元素 `A(i, j)` 

> 做完后感觉本次作业是对前两节课的一次很好的知识点梳理与复习，对Transformation的实际理解更加深刻了！√
>
> 过程理解：先架好相机，然后有个视角 得到个视锥，然后透视投影到标准立方体上，然后视口投影到屏幕上

### Code

```c++
Eigen::Matrix4f get_model_matrix(float rotation_angle)
{
    Eigen::Matrix4f model = Eigen::Matrix4f::Identity();

    // TODO: Implement this function
    // Create the model matrix for rotating the triangle around the Z axis.
    // Then return it.
    float a = (rotation_angle / 180.0) * MY_PI;
    model << 
        cos(a), -sin(a), 0, 0,
        sin(a), cos(a), 0, 0,
        0, 0, 1, 0,
        0, 0, 0, 1;

    return model;
}

Eigen::Matrix4f get_projection_matrix(float eye_fov, float aspect_ratio,float zNear, float zFar)
{
    // Students will implement this function

    Eigen::Matrix4f projection = Eigen::Matrix4f::Identity();

    // TODO: Implement this function
    // Create the projection matrix for the given parameters.
    // Then return it.
    zFar = -zFar;
    zNear = -zNear;
    Eigen::Matrix4f MPO = Eigen::Matrix4f::Identity();
    MPO << zNear, 0, 0, 0,
        0, zNear, 0, 0,
        0, 0, zNear + zFar, -zFar* zNear,
        0, 0, 1, 0;
    float n = -zNear;
    eye_fov = eye_fov*MY_PI/180;
    float t = tan(eye_fov / 2) * abs(n);
    float r = aspect_ratio * t;
    float l = -r;
    float b = -t;
    Eigen::Matrix4f MOR = Eigen::Matrix4f::Identity();
    MOR << 
        2 / (r - l), 0, 0, 0,
        0, 2 / (t - b), 0, 0,
        0, 0, 2 / (zNear - zFar), 0,
        0, 0, 0, 1;
    Eigen::Matrix4f MOT = Eigen::Matrix4f::Identity();
    MOT << 
        1, 0, 0, -(r + l) / 2,
        0, 1, 0, -(t + b) / 2,
        0, 0, 1, -(zFar + zNear) / 2,
        0, 0, 0, 1;
    projection = MOR * MOT * MPO;
    return projection;
}

Eigen::Matrix4f get_rotation(Vector3f axis, float angle) {
    float a = MY_PI * angle / 180;
    Eigen::Matrix3f R = Eigen::Matrix3f::Identity();
    Eigen::Matrix3f I = Eigen::Matrix3f::Identity();
    Eigen::Matrix3f N = Eigen::Matrix3f::Identity();
    N << 0, -axis[2], axis[1],
        axis[2], 0, -axis[0],
        -axis[1], axis[0], 0;
    R = cos(a) * I + (1 - cos(a)) * axis * axis.transpose() + sin(a) * N;
    Eigen::Matrix4f rotation = Eigen::Matrix4f::Identity();
    rotation << 
        R(0, 0), R(0, 1), R(0, 2), 0,
        R(1, 0), R(1, 1), R(1, 2), 0,
        R(2, 0), R(2, 1), R(2, 2), 0,
        0, 0, 0, 1;
    return rotation;
}
```

### 编译

```shell
mkdir build  // 创建build文件夹以保留的工程文件。
cd build     // 进入build文件夹。
cmake ..     // 通过提供CMakeLists.txt文件的路径
		     // 作为参数来运行CMake。
make −j4     // 通过make编译代码， −j4 表示通过
		     // 4个内核进行并行化编译。
./Rasterizer // 运行代码。
```