#  阈值分割
图像阈值分割，其实就是寻找一个分界值将图像像素分为两类，大于阈值的归于一类，小于阈值的归于另一类，阈值分割特别适用于目标与背景占据不同灰度级范围的图像，这样能够很好地将目标从图像中分割出来。 
阈值分割技术最重要的就是如何寻找合适的阈值，使得分割的效果达到最好，一般常用的阈值分割技术有如下几种：

-  固定阈值分割
- ostu大津算法
- 自适应阈值分割
下面我们分别进行介绍：
##  阈值分割技术
###  固定阈值分割
所谓固定阈值分割就是直接指定一个阈值，然后将所有的像素值分为两类。采用固定阈值分割通常要绘制出图像直方图，根据直方图来确定阈值，如果绘制出直方图发现图像像素分布出现双峰，那么可以采用固定阈值进行分割。
###  ostu大津算法
ostu算法由日本学者提出，能够自动计算将图像像素分开的最佳像素，下面我们来看一下ostu的推导过程：
令{0,1,2....L-1}表示一幅大小为$M*N$的数字图像中L个不同的灰度级，$n_i$表示灰度级为$i$的像素数，则图像中的像素总数为$MN=\sum_{i=0}^{L-1}n_i$，对直方图进行归一化可以得到每个灰度级的概率$p_i=n_i/MN$，并且有$$\sum_{i=0}^{L-1}p_i=1$$现在我们选取一个阈值K,并将所有的像素分为两类$C_1$与$C_2$，其中像素被分到$C_1$的概率为$$p_1(k)=\sum_{i=0}^{k}p_i$$
分到另一类的概率为$$p_2(k)=1-p_1(k)$$
分配到$C_1$的平均灰度值为$$m_1(k)=\sum_{i=0}^{k}ip_i$$
类似地，另一类平均灰度值为$$m_2(k)=\sum_{i=k+1}^{L-1}ip_i$$
灰度级K的累加均值为$$m(k)=\sum_{i=0}^{k}ip_i$$
全局图像的累加均值为$$m_G=\sum_{i=0}^{L-1}ip_i$$
类间方差定义为$$\sigma_B^2=p_1(m_1-m_G)^2+p_2(m_2-m_G)^2$$
整理上式可以得到$$\sigma_B^2=\frac{(m_Gp_1-m)^2}{p_1(1-p_1)}$$
###  自适应阈值分割
对于一些亮度不均匀的图片，采用统一的阈值可能会导致一些较亮的地方细节无法显示，这时候我们可以采用自适应阈值，在图片的不同区域采用不同的局部阈值。对于局部阈值的确定，我们可以通过计算该区域的均值、中值、加权平均等等来确定。 
##  代码实现
```
import cv2  
import matplotlib.pyplot as plt  
  
path = 'work3data/lena.jpg'  
img = cv2.imread(path,0)

 #固定阈值法  
ret1, th1 = cv2.threshold(img, 100, 255, cv2.THRESH_BINARY)  
  
 #Otsu阈值法  
ret2, th2 = cv2.threshold(img, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)

 #自适应阈值 
th3 = cv2.adaptiveThreshold(  
    img, 255, cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY, 17, 6)  
  
plt.subplot(131),plt.imshow(th1,'gray'),plt.xticks([]), plt.yticks([])  
plt.subplot(132),plt.imshow(th2,'gray'),plt.xticks([]), plt.yticks([])  
plt.subplot(133),plt.imshow(th3,'gray'),plt.xticks([]), plt.yticks([])  
  
plt.show()
```
![效果图](https://github.com/CoderPro-young/image_processing.github.io/blob/master/images/threshold%20segmentation.JPG?raw=true)



