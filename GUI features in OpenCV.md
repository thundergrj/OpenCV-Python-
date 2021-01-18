# Getting Started with Images
导入模块：
```
import cv2 as cv
```
读入图片：
```
img = cv.imread('图片路径')
```
显示图片：
```
cv.imshow('标题', img)
cv.waitKey(0) #保持图片显示，按任意键后执行下一步。或者输入为正整数时，表示显示图片多少毫秒后再执行下一步
```
保存图片：
```
cv.imwrite('文件名', img)
```
完整示例：
```
import cv2 as cv
import sys
img = cv.imread(cv.samples.findFile("starry_night.jpg"))
if img is None:
    sys.exit("Could not read the image.")
cv.imshow("Display window", img)
k = cv.waitKey(0)
if k == ord("s"):
    cv.imwrite("starry_night.png", img)
```
# Getting Started with Videos
导入模块：
```
import numpy as np
import cv2 as cv
```




## Reference
[1] https://docs.opencv.org/master/dc/d4d/tutorial_py_table_of_contents_gui.html
