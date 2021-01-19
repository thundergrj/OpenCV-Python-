# 3 Core Operations.md
# 一 Basic Operations on Images
## 1 获取像素值
```
import numpy as np
import cv2 as cv

img = cv.imread('panggege.jpeg')

px = img[100, 100]
print(px)
```
## 2 修改像素值
```
import numpy as np
import cv2 as cv

img = cv.imread('panggege.jpeg')

img[100, 100] = [0, 0, 255] #三个值依次为B、G、R
```
**注：单独获取和修改每个像素值非常慢。如果要批量修改，配合numpy使用是最好的选择**
## 3 通过item()方法获取像素值
```
import numpy as np
import cv2 as cv

img = cv.imread(‘panggege.jpeg’)

#获取并打印(10，10)坐标处的R值
print(img.item(10,10,2))
```
## 4 通过itemset()方法修改像素值
```
import numpy as np
import cv2 as cv

img = cv.imread('panggege.jpeg')

#修改R值
img.itemset((10,10,2), 5)
print(img.item(10,10,2))
```
## 5 获取Image Properties
Image Properties包括行数、列数、通道数、数据类型、像素点个数等等。
```
import numpy as np
import cv2 as cv

img = cv.imread('panggege.jpeg')
#获取图像形状
print(img.shape)
#获取像素点个数
print(img.size)
#获取像素值的数据类型
print(img.dtype)
```
**注：img.dtype非常重要，因为大量error都是由于dtype错误导致的**
## 6 图像ROI
定义：图像ROI(Region of Interest)是指图像感兴趣区域。图像处理中，一般会先从原始图片中以方框、圆等方式提取处需要处理的区域，然后再进行图像的下一步处理。
```
import numpy as np
import cv2 as cv

img = cv.imread('panggege.jpeg')

#提取一个区域，然后复制到另一个区域中
head = img[400:1600,  1200:2000]
img[2000:3200, 1200:2000] = head 

print(img.shape)
cv.imshow('panggege', img)
cv.waitKey(0)
```

## 7 分割和融合图像通道
```
import numpy as np
import cv2 as cv

img = cv.imread('panggege.jpeg')

#利用split函数分割图像通道。注意split函数比较消耗计算资源，非必要不使用，最好还是用numpy indexing
b, g, r = cv.split(img)
#利用merge函数融合图像通道，注意顺序是B、G、R，因为OpenCV使用的颜色空间是BGR
img = cv.merge((b,g,r))
cv.imshow('panggege', img)
cv.waitKey(0)
```
## 8 制造图像边界
主要掌握cv.copyMakeBorder()函数。该函数除制造边界外，还能用于convolution、padding等等。该函数参数如下：
* **src**: 输入图像
* **top, bottom, left, right**: 四个边界的宽度
* **borderType**: 给定边界类型，有如下五种预设值：
	* cv.BORDER_CONSTANT：添加颜色为定值的边界
	* cv.BORDER_REFLECT：不好描述，看运行效果就懂了，下同。
	* cv.BORDER_REFLECT_101
	* cv.BORDER_REPLICATE
	* cv.BORDER_WRAP
* **value**: 若borderType=cv.BORDER_CONSTANT，则需给定该颜色值，否则不需给定
例子如下：
```
import numpy as np
import cv2 as cv
from matplotlib import pyplot as plt

img1 = cv.imread('panggege.jpeg')

#由于matplotlib使用的颜色空间为RGB，OpenCV为BGR，所以这里定义是Blue，但显示出来是Red
BLUE = [255,0,0]

replicate = cv.copyMakeBorder(img1,10,10,10,10,cv.BORDER_REPLICATE)
reflect = cv.copyMakeBorder(img1,10,10,10,10,cv.BORDER_REFLECT)
reflect101 = cv.copyMakeBorder(img1,10,10,10,10,cv.BORDER_REFLECT_101)
wrap = cv.copyMakeBorder(img1,10,10,10,10,cv.BORDER_WRAP)
constant= cv.copyMakeBorder(img1,10,10,10,10,cv.BORDER_CONSTANT,value=BLUE)
plt.subplot(231),plt.imshow(img1,'gray'),plt.title('ORIGINAL')
plt.subplot(232),plt.imshow(replicate,'gray'),plt.title('REPLICATE')
plt.subplot(233),plt.imshow(reflect,'gray'),plt.title('REFLECT')
plt.subplot(234),plt.imshow(reflect101,'gray'),plt.title('REFLECT_101')
plt.subplot(235),plt.imshow(wrap,'gray'),plt.title('WRAP')
plt.subplot(236),plt.imshow(constant,'gray'),plt.title('CONSTANT')
plt.show()
```















