# 6.5 Opencv-python介绍

### 学习目标

- 目标
  - 掌握opencv视频读取处理API
  - 掌握opencv本地视频读取
  - 掌握opencv画矩形、圆形、文本操作API
- 应用
  - 无

## 6.5.1 opencv介绍

OpenCV是Intel®开源计算机视觉库。它由一系列 C 函数和少量 C++ 类构成，实现了图像处理和计算机视觉方面的很多通用算法。OpenCV 拥有包括 300 多个C函数的跨平台的中、高层 API。它不依赖于其它的外部库——尽管也可以使用某些外部库。

* OpenCV包含如下几个部分：
* Cxcore：一些基本函数（各种数据类型的基本运算等）。
* Cv：图像处理和计算机视觉功能（图像处理，结构分析，运动分析，物体跟踪，模式识别，摄像机定标）
* Ml：机器学习模块，目前内容主要为分类器。
* Cvaux：一些实验性的函数（ViewMorphing，三维跟踪，PCA，HMM）
* Highgui：用户交互部分，（GUI，图象视频I/O，系统调用函数）

```python
import cv2
```

### 6.5.1.2 cv2 helloword

* 摄像头捕获视频处理成灰色

```python
import numpy as np
import cv2
cap = cv2.VideoCapture(0)
while(True):
	# 一帧帧读取摄像头内容
    ret , frame = cap.read()
    
    # 显示转换后的颜色到窗口中
    cv2.imshow('frame', frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):  # 按q键退出
    	break
    
# 释放capture资源
cap.release()
cv2.destroyAllWindows()
```

注：为什么使用一下代码：

* A bug is known to occur on Linux when OpenCV uses GTK as its backend GUI library.

```
if cv2.waitKey(1) & 0xFF == ord('q'):  # 按q键退出
    	break
```

## 6.5.2 cv2视频读取处理

### 6.5.2.1 摄像头捕获视频
* cv2.VideoCapture()
  * 0：默认计算机默认摄像头
  * return:VideoCapture类

```python
cap = cv2.VideoCapture(0)

<VideoCapture 0x10bd83ed0>
```

* cap.read():读取摄像头捕获内容
  * return:ret true or false, frame:每一帧数据

```python
# 一帧帧读取摄像头内容
ret, frame = cap.read()
print(ret, frame)

True [[[ 74 124 144]
  [ 77 127 148]
  [ 74 128 151]
  ...
  ...
  [111 112 118]
  [111 114 121]
  [111 114 121]]]

(720, 1280, 3)
```

通过循环来读取摄像头捕获的内容

```python
while True:
    # 一帧帧读取摄像头内容
    ret, frame = cap.read()
    print(ret, frame)
```

注：防止出现问题，会用cap.isOpened()来检查是否成功初始化了，返回值是True即可以运行

```python
while cap.isOpened():
```

* cv2.imshow('frame', frame):显示图片

```
cv2.imshow('frame', frame)
```

* cap.release():释放资源
* cap.destroyAllWindows():关闭窗口

### 6.5.2.2 获取本地视频

* cv2.VideoCapture(file_path)

```python
import cv2
cap=cv2.VideoCapture('filename.mp4')#文件名及格式
while True:
    # 一帧帧读取摄像头内容
    ret, frame = cap.read()
    print(frame.shape)

    # 显示转换后的颜色到窗口中
    cv2.imshow('frame', frame)

    if cv2.waitKey(1) & 0xFF == ord('q'):  # 按q键退出
        break

# 释放capture资源
cap.release()
cv2.destroyAllWindows()
```

## 6.5.3 cv2 颜色空间变换

在 OpenCV 中有 超过150 种进行颜色空间转换的方法。OpenCV默认的颜色顺序是BGR，所以需要转换

* 对于BGR↔Gray的转换，使用的ﬂag是cv2.COLOR_BGR2GRAY
* 对于BGR↔HSV的转换， 使用的ﬂag是cv2.COLOR_BGR2HSV
* 对于BGR↔RGB的转换，使用的ﬂag是cv2.COLOR_BGR2RGB

注：HSV都是一种将RGB色彩模型中的点在圆柱坐标系中的表示法，HSV即色相、饱和度、明度。H（色彩/色度）的取值范围是 [0，179]， S（饱和度）的取值范围 [0，255]，V（亮度）的取值范围 [0，255]

* cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
  * return: frame

```python
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
rgb = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
```

## 6.5.4 cv2画图函数

### 6.5.4 画矩形
* cv2.rectangle(img, (xmin, ymin), (xmax,ymax), (0,255,0), 1)
  * 左上角顶点和右下角顶点的坐标
  * 画框颜色: RGB颜色
  * 画线的粗细：不设置，有默认值1

```python
cv2.rectangle(frame, (350, 0), (500, 250), (0, 255, 0), 3)
```

* cv2.circle (img,(380,0),63,(255,0,0),3)
  * 圆心与半径，显示颜色以及粗细
  * 线的粗细：-1：填充圆内部，其他值为粗细

```python
# 画出空心圆
cv2.circle(frame, (380, 0), 63, (0, 0, 255), 1)
# 填充内部
cv2.circle(frame, (380, 0), 63, (0, 0, 255), -1)
```

* cv2.putText(img, 'python', (100,100), font, 4, (255,255,255),2,cv2.LINE_AA)  

  * 在图片上添加文字,需要设置，文字内容，绘制的位置，字体类型、大小、颜色、粗细、类型

  * 字体

    * ```
      CV_FONT_HERSHEY_SIMPLEX normal size sans-serif font
      CV_FONT_HERSHEY_PLAIN small size sans-serif font
      CV_FONT_HERSHEY_DUPLEX normal size sans-serif font (more complex than CV_FONT_HERSHEY_SIMPLEX )
      CV_FONT_HERSHEY_COMPLEX normal size serif font
      CV_FONT_HERSHEY_TRIPLEX normal size serif font (more complex than CV_FONT_HERSHEY_COMPLEX )
      CV_FONT_HERSHEY_COMPLEX_SMALL smaller version of CV_FONT_HERSHEY_COMPLEX
      CV_FONT_HERSHEY_SCRIPT_SIMPLEX hand-writing style font
      CV_FONT_HERSHEY_SCRIPT_COMPLEX more complex variant of CV_FONT_HERSHEY_SCRIPT_SIMPLEX
      ```

  * 画线类型：可使用linetype=cv2.LINE_AA

    * Type of the line:
      - **8** (or omitted) - 8-connected line.
      - **4** - 4-connected line.
      - **CV_AA** - antialiased line.

```python
cv2.putText(frame, 'python', (100, 100), cv2.FONT_HERSHEY_SIMPLEX, 4, (255, 255, 255), 2, cv2.LINE_AA)
```





