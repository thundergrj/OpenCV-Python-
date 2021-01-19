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
## 3 
