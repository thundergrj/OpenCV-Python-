# 4 Smoothing Images 图像平滑

**本节目标**：

* 用不同低通滤波器模糊图像
* 对图像应用自定义滤波器（如2D卷积核）

# 4.1 2D卷积（图像滤波）

高通滤波器用于获取图像边缘，低通滤波器用于图像去噪。示例代码自定义了一个平均值滤波器：

```python
import numpy as np
import cv2 as cv
import matplotlib.pyplot as plt

img = cv.imread('p2.png')
#自定义一个平均值滤波器
kernel = np.ones((5,5), np.float32)/25
dst = cv.filter2D(img, -1, kernel)

#这里逗号分隔与换行的作用一样，换行就去掉逗号
plt.subplot(121), plt.imshow(img), plt.title('Original')
plt.xticks([]), plt.yticks([])
plt.subplot(122), plt.imshow(dst), plt.title('Averaging Filter')
#plt.xticks([]), plt.yticks([])
plt.show()
```

## 4.2 图像模糊（图像平滑）

### 4.2.1 均值滤波

均值滤波原理很简单。示例代码如下：

```python
import numpy as np
import cv2 as cv
import matplotlib.pyplot as plt

img = cv.imread('p2.png')
#使用cv模块自带的均值滤波器
blur = cv.blur(img, (5,5))

plt.subplot(121), plt.imshow(img), plt.title('Original')
plt.xticks([]), plt.yticks([])
plt.subplot(122), plt.imshow(blur), plt.title('Blur')
plt.xticks([]), plt.yticks([])
plt.show()
```

### 4.2.2 高斯滤波

高斯滤波对于滤除高斯噪声很有效。cv模块提供了两个相关函数，其中cv.GaussianBlur()用于直接对输入图像进行滤波，cv.getGaussianKernel()用于获取一个高斯核。示例代码如下：

```python
import numpy as np
import cv2 as cv
import matplotlib.pyplot as plt

img = cv.imread('p2.png')
#使用cv模块自带的高斯滤波器进行图像去噪
blur1 = cv.GaussianBlur(img, (5,5), 0.1, 0.2)

#先定义一个高斯核，再进行图像去噪
kernel = cv.getGaussianKernel(5, 0.1)
blur2 = cv.filter2D(img, -1, kernel)

plt.subplot(131), plt.imshow(img), plt.title('Original')
plt.xticks([]), plt.yticks([])
plt.subplot(132), plt.imshow(blur1), plt.title('Gaussian Blur')
plt.xticks([]), plt.yticks([])
plt.subplot(133), plt.imshow(blur2), plt.title('Gaussian Kernel')
plt.xticks([]), plt.yticks([])
plt.show()
```

### 4.2.3 中值滤波

中值滤波取滤波器所覆盖像素点的像素值的中位数作为滤波器输出，对于滤除椒盐噪声非常有效。中值滤波的kernel size必须为奇数。

```python
import numpy as np
import cv2 as cv
import matplotlib.pyplot as plt

img = cv.imread('p2.png')
#使用cv模块自带的中值滤波器进行图像去噪
blur1 = cv.medianBlur(img, 5)

plt.subplot(121), plt.imshow(img), plt.title('Original')
plt.xticks([]), plt.yticks([])
plt.subplot(122), plt.imshow(blur1), plt.title('Median Blur')
plt.xticks([]), plt.yticks([])
plt.show()
```

### 4.2.4 双边滤波

传统高斯滤波只考虑了邻域内像素的空间差异，没考虑这些像素的强度差异，因此会导致边缘模糊。双边滤波采用了两个高斯滤波器，一个考虑空间差异，一个考虑强度差异，保证只有强度差别较小的邻域内像素会进行模糊处理，这样就保留了边缘。

```python
import numpy as np
import cv2 as cv
import matplotlib.pyplot as plt

img = cv.imread('p2.png')
#使用cv模块自带的中值滤波器进行图像去噪
blur1 = cv.bilateralFilter(img, 9, 75, 75)

plt.subplot(121), plt.imshow(img), plt.title('Original')
plt.xticks([]), plt.yticks([])
plt.subplot(122), plt.imshow(blur1), plt.title('Bilateral Filter')
plt.xticks([]), plt.yticks([])
plt.show()
```

