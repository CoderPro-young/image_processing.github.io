
#  图像插值算法
图像插值在图像处理任务中应用广泛，诸如旋转额几何校正等任务中都需要使用图像插值。在这里我们介绍几种图像插值算法的原理并用它们来实现图片的放大与缩小。
##  原理介绍
### 最近邻插值
所谓插值，本质上是利用已知位置的数据去估计未知位置的数据，比如我们试图将一张图片放大两倍，那么放大后的图片中的每一个像素点可以通过比例映射到原图中的某个位置，但这个位置不一定正好落在原图中的某个像素点上，此时我们可以想到利用与该位置距离最近的像素点像素来估计该位置的像素，这就是所谓的最近邻插值。
### 双线性插值

最近邻插值较为简单，运算量不大，但在实际中可能会出现马赛克效应影响效果，这里我们介绍一种效果更好的算法即双线性插值。学过数值分析这门课的读者应该都学过线性插值算法，比如我们有一天中整点时刻的室内温度，此时我们想知道九点半时的室内温度，我们就可以利用九点与十点的温度做一个线性插值。
图像在计算机中的存储方式是二维数组（以灰度图为例），因此我们要估计某个位置的像素就需要进行多次插值。
![双线性插值](https://upload.wikimedia.org/wikipedia/commons/e/e7/Bilinear_interpolation.png)
如图所示，我们首先对上面两个顶点进行插值得到R2的像素，再对下面两个顶点做插值得到R1的像素，最后对R1和R2进行插值得到P点的像素值，我们就完成了操作，数学公式如下：
![enter image description here](https://lh3.googleusercontent.com/D2ho9d-IWVY8mVyescHtPwPO3t_xUdYXYEQzFvWoCq-5HtNhmQIIBgLfqGyKQbyXwYUnh73IPxCYYV6W6JpGzz-xLYwD1U09aABlAJ6BT1FagPdqvZZXpYVYuUbp5_7XZhW72abXzx1d1WY4OVv7n-5tcyO791ZMJxVwX20Nm26deCSAObV_4YvsowEz8ztwXMwnG3o8e8GlODgeBrycSYwBXDVsB7vS8YE9XVgXshKhhGc193hTFiO0MgnB06tkKMWu_O06I7dFN3r6jehjqfRlHWDK6kQTugTxdKxik347Ycd-0YHd0DdYKWbnWltCPvLJbbrKex3-lc8wz2DaFjf2QwMoOR0WF85w8k_gm01y2bGP5tD3UPU8T1RaJoB62EoDf0hZjH-ZSCy6N32x0iBAf4Zm3dXGqIeGC6qaB6nuEIbsoVsLGkRRrriMv-lc_n5n1hLbE3D9PGq1JZKaBQNvqL8RAYWlZNhCKlWN7seZDaab3260ZlJChyv4U6R_L42NZJjj4Ba-39NjEC1AG7LUxvFvj_59RslqOoMCvicHNMsRZpmsIgHK3AVfdhZI4W4ngaQyrcCKCyNv6Fmbl3hGcdgQMpp9vM8wSsgDIen3XHWjLKHSsHPouASvRMPtSI9Bx_rdtL_DX2vTOk35rPehUOXfysIz51p8P9u4OM891tYKdFaQG7jk_FUC=w760-h230-no)
![enter image description here](https://lh3.googleusercontent.com/AAUuVQ7syIzJYOV5wyuY4Ws-NEcfrWPJiw45Pkg2jYtIo29X1k1W1MvTYBdalWV7mcIdYo2mugdrDLce5PadNYYZiEObOELJAkc-pTUZij5b-NL9cMBhCw9dJiwv9LNWun-PsO1QNVRO6KVEKgbHRRuS0cPmXlLv-h8J6LYtSeYjAQ3FZnSO9gaFwF7kT0jUX3g3CA-vpOiC3vPA4jzhGPrh6r44LCswQu4ZsDdq-dSWCzjlM6X7JvWlNwkrzgso51b0f2CLUUIlMfJwRtegt6Fq12mRBJFq__vmUV0Z98nF9otDboTUo932pPadGhDbmXCRaAKhnTf4k8TO7xpC4eqULRK2Mr5CB-IQJhtyuXcs06Ys7BfOon27GydRVAW9wcwgUTEjWL7uQIfidmlJS72IuHVTC_TYZVXvEpyYh89P_cmUJRtF3vqAKtMFh8VPHGXUwO0m0gGPzfdKXjf5Nc2AlYaiaiXfRKgQfm7v-W2N2BebhyKgOSCMQb7HSdQAo-c0Rlx5no3ORHv9eTWtnUIIvWHfX2e5AKu4K05GOv7V4UnTn9mIJwmnZ1BjRqsl4rI9e7E1AGfmn9NyK289Mem85yyDCdouqYFy1VrCBOElG03ME77YRtuOf-HnVUhqw9ZxLqHiSq9wtm66EX7XnNXc-wNjYo6oBdW5uVLF9Ao8RiCd4ogy0cD8onzk=w514-h131-no)
读者可以自行推导，实际就是我们在中学阶段学过的两点式直线方程。
##  代码实现
我们基于python+opencv实现两种插值算法，对于双线性插值，我们还给出纯python实现的代码以便更好地理解原理。我们使用`cv2.resize()`函数来实现插值运算。![enter image description here](https://lh3.googleusercontent.com/BUojjEIeb1xBHvrdepHguGqylNDZK93alnzBNZgOGbe8qmXSz86cDvoImuAF4JABoRQlW29t1r9qjgxy0Wt0TDfcCty5m2JOxdVwV0b7kXK1vVokQVgMwAkIXP4hqSFAVui7SeLdGVsyyqAPxTdCFA9mj-1k3d2mzju5Je0vj9ih15L4ZPKhgOYnhYVuw32NPKudj0otqWYe4CgNAj2JykxSoFSQqq2qQhLoZMXXaR1oYRVOGCJi_VD6_p1f1boMZMJxBVvWvduexOvIuQkZw7x-MWoABIZBnv6PuWS-E8xYFC00MhfQCP84gW5k75jTmsx-y4yGfP9hJU5KkRfHjtegXCZmQCoxIbckYRu1Oknvjd_qVEvzAxvThsig7Ub2pl0g0I-eWXgLcMYaVai9JulcNcG405J6mSBRoBoYd2INXJBVccRyDnk4P9haz8ocuxJXKciilJvoG2AVDSyZ2x9CDa0NYgnDJLuEIKWCGJtCkzbAZyKFwm3fVfWIWhwWbM4MAem-QiT1JYui1lZAEOhCWuOPEvFrM36zn0POK-gv-CGos9-ATTA_rAL_bnLVQy3-we-5deTXht-MYGaNMgI3SUeNdVJYnJMYVW_PpNxlW-xBhEvNITp8bppAqalnbuMrauRgbmXPTXv9Kh1uFOGtguz78TV8gBreEfGmVYQqrl3Lo1_qVCZGbCrJ=w566-h604-no)
上图就是函数的参数以及用法，我们在使用时有两种方式：一是直接通过`dsize`指定插值后图像的大小（只需指定`heigh`以及`width`即可），二是令`dsize=None`，然后通过`fx fy`来指定在两个方向上缩放的比例。
```
import numpy as np  
import cv2 as cv

img = cv.imread("E:/master source/opencv-c++/Project1/lena.jpg")
#使用最近邻插值将图像放大两倍
resized_1 = cv.resize(img, dsize=(512, 512), interpolation=cv.INTER_NEAREST)  
cv.imshow('resized_1', resized_1)  
cv.waitKey(0)  
#使用双线性插值将图片缩小
scale_factor = 0.3  
resized = cv.resize(img, dsize=None, fx=scale_factor, fy=scale_factor, interpolation=cv.INTER_LINEAR)  
cv.imshow('resized', resized)  
cv.waitKey(0)
```
![enter image description here](https://lh3.googleusercontent.com/K_Pr1bEigenuqZ62pvt8z0089G05blyssdsonRC82AG4VL00PrpLaVbZV19j4FsvK6H-9bKRFIiU--CPuFqFQMCsKToQfGUUX0kfunnNDaQkaGr_pc0iWLCzw3zCHNYVOIc05wk-JAwy5BaPf7cVKMHQADHoPap_Va1_tKAHByQq6dOuauL1-sZeBTuTAEtDKVGjSkdkJD7pqoGRYArEf1LwWZwYhazYBkgDh9LhQmXiviFyt9NMI-7KPn5waFdC_Grvtu_UF8Bp6P51HCWrz4RkMUYr-_Vokfdsrp3pY4DEhI4bQnpV9zikvpD_zbbJCUDoc2uFN1_mPI3RT6AUHVhn77yoNCOqRBT9M1lDLqZU5lMgauBgtKZX3yykpW7Gz5mLKzmvsth_ZDj3isp0zjMyng5ccAycQamTqY4LU3JjihszGa1v6dPIz6rN7KlbETCGMBPeT2-BhN9lg9P7pwHcDB9F_hQrgrs3bTa2I3bkxfvfhVVElThRmLQZVQe8EzkVzT6PkapIWk1_v4YKOVKz3iil-1ppDA2dkirWlXzhK2ruH3jmer8_tvSHsuQ5kthsY73SLV_zPT9brnfGws1wVIbCjxugTrDVCkR6arbsNkgJl2feA49NsAgd3_cbLrJnRtWBqO9H8diW6mUv9lmlEevDzquhbzRCkzs3MEDUNVj0_dcC2B49nwq6=w641-h680-no)
使用最近邻插值将图像放大两倍


![enter image description here](https://lh3.googleusercontent.com/Zm9kUscHm_e4H51PRJJYwZYsiC1SSi9AQ4Uu_a85gwd2Fvx_wEkpCFHmlY7j_WhQnA4J3RicmgrE4pvZAfwjavt1ZvsuEPDhpQfaSa7kx3ZGxDFy9-R1gQkZ4zC8AmIQEO3Lop7cbgqYRPt6zvjHEPiO6Bib9oZbtts0-7xG-fTwi8Octzq4D3Gtic5J4UKufCySlFPyS1SCf-K16IbTMpuO5CmE_iO4_kfKf8z0LIdO2yJzPrNOTAz8m4JctSU8_NqckQ9lyHGBIygLcw5JNaobBPtWBdAtt1xnq38OQUYvR1E3Xtxt0RZkO3QEMbgLH40MwuY_kQOUskDUovBbZu7J_iWL5npQt6NRhTXXJxNfL29KU8JSh8gNwTKEI1TKij0tF0RiCR6Hw2njRkoiSTIfxO9EFOF_iofGaRQw7Po0Y0pc393FzjdotRxFqGZi3kSnPbbntsgmadKY8YspGyponygMPbfpysRGxGFqIf-hczvye0YXpGpHGRrV6L4zW_QCsyZnPVedrXs08CJObOtZ9kdtVlh3uZhXW5SjL_aa4-BprZmmGTRWZxYxluRWHjUckHz4lGvVofrhANF-kig8L9ptAlhzmE8n_pg50lVbpVkoR9Sxj9XmTGt0-811w4MjO4sS0ryIurx-gvmo_Vk_WwX-PBz7LiDdETdO1hXa42HBB8YhFIVVsmlH=w152-h137-no)
使用双线性插值将图片缩小。
下面我们来看看一下如何通过纯python实现双线性插值。
```
def bilinear_img(src, scale_factor):  
    (height, width, channel) = src.shape  
    dst_height = int(scale_factor * height)  
    dst_width = int(scale_factor * width)  
    dst = np.zeros((dst_height, dst_width, 3), dtype=np.uint8)  
    for dst_x in range(dst_height):  
        for dst_y in range(dst_width):  
        #几何中心重合
            src_x = (dst_x + 0.5) * (1/scale_factor) - 0.5  
		    src_y = (dst_y + 0.5) * (1/scale_factor) - 0.5  
			src_x_0 = int(src_x) if src_x<256 else 255  
			src_y_0 = int(src_y) if src_y<256 else 255  
			src_x_1 = src_x_0 + 1  
			src_y_1 = src_y_0 + 1  
		    if(src_x_1>255):  
		                  src_x_1 = 255  
		    if(src_y_1>255):  
		                  src_y_1 = 255  
  #先对上下两条线进行插值  
    
		    point1 = (src_x_1 - src_x) * src[src_x_0][src_y_0][:] + (src_x - src_x_0) * src[src_x_1][src_y_0][:]  
            point2 = (src_x_1 - src_x) * src[src_x_0][src_y_1][:] + (src_x - src_x_0) * src[src_x_1][src_y_1][:]  
            #对于插值得到的两点再进行一次插值  
  res = (src_y_1 - src_y) * point1 + (src_y - src_y_0) * point2  
            dst[dst_x][dst_y][:] = (res)  
  
    return dst
```
调用一下，看看结果如何
```
scale_factor = 1.5
dst = bilinear_img(img, scale_factor)  
cv.imshow('bilinear', dst)  
cv.waitKey(0)
```
![enter image description here](https://lh3.googleusercontent.com/uASFvI99Cfs2Z7qeobtB1vXsiHRYo9CGJ_BeWUvGGTZIBSpzEbD-zcP3jjK8-Ntxda0Wd7vlNTEfTrrjs8cdu-Kav5WJ1-MNSgTMkkasMa_B4Dn1-MpsPCX8nwbXto6BDFMgC5ZvonycBYlB3RX7KOmVk_I-OFNfjXVai9Rvg6xPS-xlbrKGltcrdLBwdE4JPgvgRMcFIGFFu97sAEDe8RvnZc14lxJ60nrJwbxI530-KZg1_J9Ri2e_xu-s-L9YHTYpXct26f1frJ137ytLmiRFSOpHdKcMCKWfRcuN29yl8Aq3qJMpk2lJGKTgNbDqWLWNSaoubzrfWQo0uko6zWPyJbaHs26Yg2-dknn5wmdFFZxYSo3Q8CTYfszBe994OrQbqPyI0QqmchRsBobcgd_vokskNIVkxMCIZH0Ao4pcTXnSXTS3ldVS3T9uHohNRr5_9tC6fTSf6BZfH5aftcxK-A0sd790eRc1wue4N40uj4tVfEctej6wQ_llqDjjajKJ8xra06eftGB2fnIx6ihCZK1i0Hnpl_TEX2TckJqFNjKbvmIiWJL492O7LBT9qwGt8ZqAGaThot6gT4S0yIArXt-2U_9sB3y2oJs57N8mIhM-3rJzffKMew1dKdBzgll31KxIdyoTadpfClRcfgSzhmiY4AD2kTxFG30I_wv3hl63DgEBzk3S53FW=w344-h363-no)

参考资料：
[图像插值算法](https://github.com/datawhalechina/team-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80%EF%BC%9A%E5%9B%BE%E5%83%8F%E5%A4%84%E7%90%86%EF%BC%88%E4%B8%8A%EF%BC%89/Task01%20%E5%9B%BE%E5%83%8F%E6%8F%92%E5%80%BC%E7%AE%97%E6%B3%95.md)
[ 双线性插值算法以及python实现](https://blog.csdn.net/pku_Coder/article/details/82690128)
