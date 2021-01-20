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
# 二 Arithmetic Operations on Images
## 1 图像相加
图像相加有两种方式，一种是使用cv.add()，是饱和运算；另一种是通过numpy运算直接相加，是模运算。需要注意的是相加的两图片需要有相同的通道数和数据类型，或者第二张图是标量。这两种方式的差别可从下述代码中看出：
```
import numpy as np
import cv2 as cv
from matplotlib import pyplot as plt

x = np.uint8([250])
y = np.uint8([10])

print(cv.add(x,y))  # 250 + 10 = 260 => 255
print(x+y)  # 250+10 = 260 % 256 = 4
```
## 2 图像融合
```
import numpy as np
import cv2 as cv

img1 = cv.imread('img1.jpg')
img2 = cv.imread('img2.jpg')

dst = cv.addWeighted(img1, 0.7, img2, 0.3, 0) # dst = 0.7*img1 + 0.3*img2 + 0

cv.imshow('dst', dst)
cv.waitKey(0)
cv.destroyAllWindows()
```
## 3 Bitwise运算
```
import cv2 as cv
# Load two images
img1 = cv.imread('messi5.jpg')
img2 = cv.imread('opencv-logo-white.png')

# I want to put logo on top-left corner, So I create a ROI
rows,cols,channels = img2.shape
roi = img1[0:rows, 0:cols]

# Now create a mask of logo and create its inverse mask also
img2gray = cv.cvtColor(img2,cv.COLOR_BGR2GRAY)
ret, mask = cv.threshold(img2gray, 10, 255, cv.THRESH_BINARY)
mask_inv = cv.bitwise_not(mask)

# Now black-out the area of logo in ROI
img1_bg = cv.bitwise_and(roi,roi,mask = mask_inv)

# Take only region of logo from logo image.
img2_fg = cv.bitwise_and(img2,img2,mask = mask)

# Put logo in ROI and modify the main image
dst = cv.add(img1_bg,img2_fg)
img1[0:rows, 0:cols ] = dst

cv.imshow('res',img1)
cv.waitKey(0)
cv.destroyAllWindows()
```
# 三 Performance Measurement and Improvement Techniques
本小节介绍如何测试算法性能，以及如何提高算法性能。
## 1 使用OpenCV模块测试
**cv.getTickCount()函数：**返回此刻时钟循环次数
**cv.getTickFrequency()函数：**返回时钟频率
```
import cv2 as cv

t1 = cv.getTickCount()
#待测试代码
t2 = cv.getTickCount()

t = (t2-t1)/cv.getTickFrequency()
```
**注：使用time模块的time.time()函数可获得一样的效果**
## 2 OpenCV默认优化函数
目前大部分CPU支持AVX指令集等，OpenCV可借此对代码进行优化。根据测试，同样的OpenCV 代码，开启优化的运行速度比不开启优化的运行速度快2倍。默认是开启了优化的，也可以利用代码手动开启或关闭。相关函数如下：
**cv.useOptimized()函数：**返回值为True或False，用于查看当前是否开启了系统优化
**cv.setUseOptimized()函数：**参数为True或False，无返回值，用于开启或关闭系统优化
## 3 在IPython中测试代码性能
## 4 OpenCV代码性能优化原则
* 在Python中尽量少用循环体，尤其避免循环嵌套。因为循环需要很大的计算量
* 尽可能将算法和代码向量化，因为numpy和OpenCV都对向量计算有优化
* 利用缓存一致性（cache coherence）
* Never make copies of an array unless it is necessary. Try to use views instead. Array copying is a costly operation.

## Reference
[1]https://docs.opencv.org/master/d7/d16/tutorial_py_table_of_contents_core.html













