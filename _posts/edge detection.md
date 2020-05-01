#  边缘检测
##  基本概念
边缘像素是图像中灰度突变的那些像素，而边缘是连接边缘像素的集合。 根据边缘处灰度会突变的特性，我们可以利用一阶导数来检测灰度变化。
###  图像梯度
为了在一幅图像的(x,y)位置处寻找边缘的强度与方向，所选择的工具就是梯度，因为我们在微积分里面都学过，对于一个二维函数，梯度方向就是函数值变化最快的方向，对于一幅图像我们自然也可以将其视为一个二维函数，函数值即为某个位置处的像素值。 
因此在图像处理中，我们通常可以通过梯度算子求出一幅图片对应的梯度图并得到每一点梯度向量的方向，某一点的边缘方向与该点的梯度向量方向正交。 
>  用于计算梯度偏导数的滤波器模板，通常称为梯度算子、差分算子、边缘算子。

###  梯度算子
我们这里介绍sobel算子，表达式为：
$$
\left[ 
\begin{matrix} 
1 & 2 & 1 \\ 
0 & 0 & 0 \\ 
-1 & -2 & -1 
\end{matrix}
   \right] 
$$
$$
\left[ 
\begin{matrix} 
1 & 0 & -1 \\ 
2 & 0 & -2 \\ 
1 & 0 & -1 
\end{matrix}
   \right] 
   $$

两个算子分别用于对竖直方向和水平方向的梯度进行检测。
###  canny 检测
canny检测由一下几步组成：

1、用一个高斯滤波器平滑输入图像
2、计算梯度幅值图像和角度图像
3、对梯度图像应用非最大抑制
4、用双阈值处理和连接分析来检测并连接边缘。
下面我们分别讲解：

第一块高斯滤波无需赘言，生成高斯滤波器模板后直接卷积运算即可。
第二步我们通过相关的滤波器模板（梯度算子）计算得到$g_x,g_y$，通过计算得到梯度幅度与方向。

第三步稍微复杂一些，我们对于一个点的边缘定义四个方向：水平、垂直、$+45^。$、$-45^。$，将它们分别表示为$d_1,d_2,d_3,d_4$我们首先找到最接近梯度方向的$d_k$，找到以后，我们比较梯度值与$d_k$的两个邻居，若梯度值小于两个邻居之一，则令$g_n(x,y)=0$，否则令$g_n(x,y)=M(x,y)$,这里的$g_n(x,y)$为非最大抑制之后的图像。

第四步可以用下面的一张图来概括：
![enter image description here](https://github.com/2209520576/CV-Image-Processing/blob/master/IMG/doubleThreshold.png?raw=true)

##  代码实现
```
import cv2  
import numpy as np  
import matplotlib.pyplot as plt  
  
img = cv2.imread('data/Fig1016(a)(building_original).tif', 0)  
cv2.imshow('original',img)  
cv2.waitKey(0)  
  
sobel_x = cv2.Sobel(img, -1, 1, 0)  
sobel_y = cv2.Sobel(img, -1, 0, 1)  
  
plt.subplot(121), plt.imshow(sobel_x, 'gray'), plt.xticks([]), plt.yticks([])  
plt.subplot(122), plt.imshow(sobel_y, 'gray'), plt.xticks([]), plt.yticks([])  
plt.show()  
  
edges = cv2.Canny(img, 30, 150)  # canny边缘检测  
print(edges.shape)  
  
cv2.imshow('canny', edges)  
cv2.waitKey(0)
```
