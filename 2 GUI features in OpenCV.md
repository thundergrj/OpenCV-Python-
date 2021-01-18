# 一 Getting Started with Images
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
# 二 Getting Started with Videos
## 1 从文件导入视频：
```
import numpy as np
import cv2 as cv

cap = cv.VideoCapture('文件路径') #VideoCapture()是用于从视频文件、图片序列、摄像头捕获视频的类

while cap.isOpened():  #isOpened()方法检测视频是否能成功打开。成功打开时，返回True，否则返回False
    ret, frame = cap.read()  #read()方法读取一帧图片，并返回两个值。第一个值为True或False，表示是否读到图片；第二个值为一帧图片
    
    #if frame is rad correctly ret is True
    #若frame到了最后一帧，ret为False，进入下面的if，break成功，跳出while循环
    if not ret:
        print('Cannot receive frame (stream end?). Exiting...')
        break
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    
    cv.imshow('frame', gray)
    #ord('q')返回字母q对应的ASCII码数值，即113.本句指在视频没有播放完之前，按q键就会立即退出
    if cv.waitKey(1) == ord('q'):  
        break
        
# 当跳出while循环体后，释放cap对象的空间
cap.release()
cv.destroyAllWindows()
```
## 2 从相机捕捉视频：
```
import numpy as np
import cv2 as cv
cap = cv.VideoCapture(0)  #表示建立一个从编号为0的相机处捕捉图像的对象
if not cap.isOpened():
    print("Cannot open camera")
    exit()
while True:
    # Capture frame-by-frame
    ret, frame = cap.read()
    # if frame is read correctly ret is True
    if not ret:
        print("Can't receive frame (stream end?). Exiting ...")
        break
    # Our operations on the frame come here
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    # Display the resulting frame
    cv.imshow('frame', gray)
    if cv.waitKey(1) == ord('q'):
        break
# When everything done, release the capture
cap.release()
cv.destroyAllWindows()
```
## 3 保存视频：
```
import numpy as np
import cv2 as cv
cap = cv.VideoCapture(0)
width = int(cap.get(cv.CAP_PROP_FRAME_WIDTH))  #获取捕捉到图像的width
height = int(cap.get(cv.CAP_PROP_FRAME_HEIGHT))  #获取捕捉到图像的height
# Define the codec and create VideoWriter object
fourcc = cv.VideoWriter_fourcc(*'XVID')  #fourcc是一种独立标识视频数据流格式的四字符代码。Mac下要将XVID改为MJPG，视频才能保存成功
out = cv.VideoWriter('output.avi', fourcc, 20.0, (width,  height))  #实例化输出视频对象，文件名为output.avi，标识符为fourcc，帧率为20，分辨率需要设置为与输入一致，否则无法保存成功
while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        print("Can't receive frame (stream end?). Exiting ...")
        break
    out.write(frame)
    cv.imshow('frame', frame)
    if cv.waitKey(1) == ord('q'):
        break
# Release everything if job is finished
cap.release()
out.release()
cv.destroyAllWindows()
```
# 三 Drawing Functions in OpenCV



## Reference
[1] https://docs.opencv.org/master/dc/d4d/tutorial_py_table_of_contents_gui.html
