**需要大量线性代数和离散数学的知识！！！**

# 图像入门

## 读取图像

### cv.imread(argu1,argu2)

argu1为图片路径

argu2为flag，指定读取图像方式

对于常见的RGB图像，返回的是一个蓝色，绿色，红色值的数组，维度大小为3.而对于灰度图像，仅返回相应的强度。

| 参数                         | 说明             |
| -------------------------- |:--------------:|
| **cv.IMREAD_COLOR/0**      | 加载彩色图像         |
| **cv.IMREAD_GRAYSCALE/1**  | 灰度模式           |
| **cv.IMREAD_UNCHANGED/-1** | 加载图像,包括alpha通道 |

```python
import numpy as np
import cv2 as cv
# 用灰度模式加载图像
img = cv.imread('messi5.jpg', 0)
```

## 显示图像

### cv.imshow(argu1,argu2)

argu1为窗口名,是一个字符串

argu2是我们读取的图像

```python
cv.imshow('image', img)
cv.waitKey(0)
cv.destroyAllWindows()
```

### cv.waitKey(argu)

argu是以毫秒为单位的时间,该函数为任意键盘事件等待指定毫秒.如果是0,会一直等待按键按下.可以设定特定的按键。如果是64位机器，需要修改代码为：cv.waitKey(augu) & 0xFF

### cv.destroyAllWindows()

销毁所有创建的窗口

### cv.destroyWindow(argu)

销毁指定窗口,argu为窗口名

### cv.namedWindow(argu)

创建一个窗口,随后可以将图像加载到该窗口

| 参数                 | 说明      |
| ------------------ | ------- |
| cv.WINDOW_AUTOSIZE | 自动尺寸    |
| cv.WINDOW_NORMAL   | 能调整窗口大小 |

## 保存图像

### cv.imwrite(argu1，argu2)

argu1为文件名

argu2为保存的图像

路径为工作目录

## 视频入门

## 从相机捕获视频

### cv.VideoCapture(argu)

argu为设备索引（0，1，等）或视频文件名，这个可以初始化一个VideoCapture对象，然后才能调用成员函数。例如初始化一个VideoCapture对象cap。下面是调用成员函数例子。

### cap.read()

按帧捕获，返回一个bool值

```python
ret,frame=cap.read()
```

frame为捕获帧，ret为bool值

### cap.isOpened()

检查cap对象是否初始化，返回一个bool值。

### cap.open()

如果cap对象没有初始化成功，该方法可以打开

### cap.get(propld)

propld是一个0-18的数字，每个数字代表视频的一个特征

## 播放视频文件

```python
import numpy as np
import cv2 as cv
cap = cv.VideoCapture('vtest.avi')
while(cap.isOpened()):
    ret, frame = cap.read()
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    cv.imshow('frame',gray)
    if cv.waitKey(1) & 0xFF == ord('q'):
        break
cap.release()
cv.destroyAllWindows()
```

## 保存视频

### cv.VideoWriter(argu1,argu2,argu3,argu4，argu5)

创建一个VideoWriter对象，然后才能使用成员函数

argu1是保存视频的名称

argu2是视频编码器，一般是一个FourCC对象

argu3是每秒帧数

argu4是帧大小

argu5是isColor flag，True就是彩色帧，否则就是灰度帧

### cv.VideoWriter_fourcc(argu1)

创建一个FourCC对象，指定视频编码器

argu是可用编码的[列表]([Best Online Casinos - Four Countries Casinos (fourcc.org)](https://fourcc.org/))

```python
import numpy as np
import cv2 as cv
cap = cv.VideoCapture(0)
# 声明编码器和创建 VideoWrite 对象
fourcc = cv.VideoWriter_fourcc(*'XVID')
out = cv.VideoWriter('output.avi',fourcc, 20.0, (640,480))
while(cap.isOpened()):
    ret, frame = cap.read()
    if ret==True:
        frame = cv.flip(frame,0)
        # 写入已经翻转好的帧
        out.write(frame)
        cv.imshow('frame',frame)
        if cv.waitKey(1) & 0xFF == ord('q'):
            break
    else:
        break
# 释放已经完成的工作
cap.release()
out.release()
cv.destroyAllWindows()
```

# 图像基本操作

## 访问和修改像素值

### shape成员

```python
img=cv2.imread('pic')
h,w,c=img.shape
```

h为高，w为宽，c为通道数。它返回一组由图像的行、列和通道组成的元组（如果图像是彩色的）。如果图像是灰度，则返回的元组仅包含行数和列数，因此可以用来检测图像是灰度图还是彩色图。

## 访问像素值

img[x,y]

返回x，y处的RGB数组

img[x,y,c]

访问x，y处的c通道值，c可为0，1，2。分别为G,B,R

- 注：上述方法通常用于选择数组某个区域。对于单个像素的访问，可以选择Numpy数组方法中的array.item()和array.itemset()，返回值是一个标量。如果需要访问所有的G,R,B值，需要分别为所有的调用array.item()，如：

```python
#访问 红色通道 的值
img.item(10,10,2)

#修改 红色通道 的值
img.itemset((10,10,2),100)
img.item(10,10,2)
```

## 访问图像属性 shape&dtype

图像属性包括行数，列数和通道数，图像数据类型，像素数等

### .shape

一般通过.shape访问图像形状。返回值为h，w

### .dtype

一般通过.dtype来访问图像数据类型，代码中的大量错误是由无效的数据类型引起的。

## 图像中的感兴趣区域（ROI）

确定搜索目标的大致范围，然后将其复制到另一个区域再进行搜索，也就是逐步搜索的操作，这样可以提高准确性和性能。

```python
ball = img[280:340,330:390]
img[273:333,100:160] = ball
```

## 拆分和合并图像通道

### cv.spilt(img)

```python
b,g,r=cv.split(img)
```

这是一个非常消耗时间的操作。非必要时使用Numpy索引

### numpy.array的切片方法

```python
b = img[:,:,0]
```

### cv.merge((b,g,r))

```python
img = cv.merge((b,g,r))
```

## 制作图像边界（填充）

### cv.copyMakeBorder()

该函数采用以下参数：

- src-输入的图像

- top,bottom,left,right-上下左右四个方向上的边界拓宽的值

- borderType-定义要添加的边框类型的标志

| flag                                    | 说明                            |
| --------------------------------------- | ----------------------------- |
| cv.BORDER_CONSTANT                      | 添加一个恒定的彩色边框。该值应作为下一个参数value给出 |
| cv.BORDER_REFLECT                       | 边框将是边框元素的镜像反射                 |
| cv.BORDER_REFLECT_101或cv.BORDER_DEFAULT | 与上面相同，但略有改动                   |
| cv.BORDER_REPLICATE                     | 最后一个元素被复制                     |
| cv.BORDER_WRAP                          | 不好解释                          |

演示：

```python
import cv2 as cv
import numpy as np
from matplotlib import pyplot as plt
BLUE = [255,0,0]
img1 = cv.imread('opencv-logo.png')
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

结果，图像是通过 matplotlib 展示的。因此红色和蓝色通道将互换：

<img title="" src="https://opencv.apachecn.org/docs/4.0.0/img/border.jpg" alt="" data-align="inline"> 

# 图像的算术运算

## 图像加法

### cv.add(img1,img2)

两个图像应该具有相同的深度和类型，第二个图像可以是像素值。OpenCV添加是饱和操作，对于结果溢出的数据，会通过某种方式使其在限定的数据范围内。

## 图像混合

### cv.addWeighted(img1,weight1,img2,weight2,gamma)

对图像赋予不同的权重，从而给出混合感和透明感，图像按以下等式添加：

$$
q(x)=(1-\alpha)f_0+\alpha f_1(x)
$$

通过（0，1）之间改变的值来设置图像权重

参数gamma为亮度

```python
img1= cv.imread('img1')
img2= cv.imread('img2')

dst = cv.addWeighted(img1,0.7,img2,0.3,0)

cv.imshow('dst',dst)
cv.waitKey(0)
cv.destroyAllWindows()
```

## 按位操作

```python
#加载两张图片
img1 = cv.imread('messi5.jpg')
img2 = cv.imread('opencv-logo-white.png')
#我想在左上角放置一个logo，所以我创建了一个 ROI,并且这个ROI的宽和高为我想放置的logo的宽和高
rows,cols,channels = img2.shape
roi = img1 [0:rows,0:cols]
#现在创建一个logo的掩膜，通过对logo图像进行阈值，并对阈值结果并创建其反转掩膜
img2gray = cv.cvtColor(img2,cv.COLOR_BGR2GRAY)
ret,mask = cv.threshold(img2gray,10,255,cv.THRESH_BINARY)
mask_inv = cv.bitwise_not(mask)
#现在使 ROI 中的徽标区域变黑
img1_bg = cv.bitwise_and(roi,roi,mask = mask_inv)
#仅从徽标图像中获取徽标区域。
img2_fg = cv.bitwise_and(img2,img2,mask = mask)
#在 ROI 中放置徽标并修改主图像
dst = cv.add(img1_bg,img2_fg)
img1 [0:rows,0:cols] = dst
cv.imshow('res',img1)
cv.waitKey(0)
cv.destroyAllWindows()
```

效果图：左边为创建的蒙版，右边为最终结果。

![](https://opencv.apachecn.org/docs/4.0.0/img/overlay.jpg)

## 按位取反操作

### cv.bitwise_not(img)

![](https://img-blog.csdnimg.cn/20210614215156126.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ4MDk1ODQx,size_16,color_FFFFFF,t_70)

## 按位与操作

### cv.bitwise_and(argu1,argu2,argu3)

argu1为目标图片

argu2为源图片

argu3为掩膜

## 掩膜

mask就是位图，与原图像进行位运算，也就是说选择哪个像素允许拷贝，哪个像素不允许拷贝。

# 图像处理

## 颜色空间转换

### cv.cvtColor(img,flag)

flag决定了转换的类型，如下：

从BGR->Gray：cv.COLOR_BGR2GRAY

从BGR->HSV：cv.COLOR_BGR2HSV

## 目标追踪

HSV比BGR在颜色空间上更容易表示颜色。常用来追踪对象

方法如下：

- 提取视频每一帧

- BGR转换为HSV颜色空间

- 用特定像素范围对HSV图片做阈值

- 提取对象

```python
import cv2 as cv
import numpy as np
cap = cv.VideoCapture(0)
while(1):
    # Take each frame
    re, frame = cap.read()
    # Convert BGR to HSV
    hsv = cv.cvtColor(frame, cv.COLOR_BGR2HSV)
    # define range of blue color in HSV
    lower_blue = np.array([110,50,50])
    upper_blue = np.array([130,255,255])
    # Threshold the HSV image to get only blue colors
    mask = cv.inRange(hsv, lower_blue, upper_blue)
    # Bitwise-AND mask and original image
    res = cv.bitwise_and(frame,frame, mask= mask)
    cv.imshow('frame',frame)
    cv.imshow('mask',mask)
    cv.imshow('res',res)
    k = cv.waitKey(5) & 0xFF
    if k == 27:
        break
cv.destroyAllWindows()
```

效果图：

![图片](https://opencv.apachecn.org/docs/4.0.0/img/4-1-1.jpg)

## 如何找到HSV值

不需要输入图片，只需要输入需要的BGR值，转换就行

```python
>>> green = np.uint8([[[0,255,0 ]]])
>>> hsv_green = cv.cvtColor(green,cv.COLOR_BGR2HSV)
>>> print( hsv_green )
[[[ 60 255 255]]]
```

## 图像阈值

### cv.threshhold(argu1,argu2,arug3,argu4)

argu1为原图像，一般为灰度图

argu2对像素值进行分类的阈值

argu3为当像素值高于阈值时应该被赋予的新像素值

argu4为阈值方法，包括：

| 阈值方法                 | 说明                      |
| -------------------- | ----------------------- |
| cv.THRESH_BINARY     | 超过阈值部分取maxval（最大值），否则取0 |
| cv.THRESH_BINARY_INV | THRESH_BINARY的反转        |
| cv.THRESH_TRUNC      | 大于阈值部分设为阈值，否则不变         |
| cv.THRESH_TOZERO     | 大于阈值部分不改变，否则设为0         |
| cv.THRESH_TOZERO_INV | THRESH_TOZERO的反转        |

```python
import cv2 as cv
import numpy as np
from matplotlib import pyplot as plt
img = cv.imread('gradient.png',0)
ret,thresh1 = cv.threshold(img,127,255,cv.THRESH_BINARY)
ret,thresh2 = cv.threshold(img,127,255,cv.THRESH_BINARY_INV)
ret,thresh3 = cv.threshold(img,127,255,cv.THRESH_TRUNC)
ret,thresh4 = cv.threshold(img,127,255,cv.THRESH_TOZERO)
ret,thresh5 = cv.threshold(img,127,255,cv.THRESH_TOZERO_INV)
titles = ['Original Image','BINARY','BINARY_INV','TRUNC','TOZERO','TOZERO_INV']
images = [img, thresh1, thresh2, thresh3, thresh4, thresh5]
for i in xrange(6):
    plt.subplot(2,3,i+1),plt.imshow(images[i],'gray')
    plt.title(titles[i])
    plt.xticks([]),plt.yticks([])
plt.show()
```

效果图：

![图片](https://opencv.apachecn.org/docs/4.0.0/img/Image_Thresholding_1.png)

## 图像变换

# 特征检测与描述

## 蛮力匹配(Brute Force)

### cv.BFMatcher(normType,crossCheck)

该方法可以创建一个BFMatcher对象。参数normTYPE指定要使用的距离测量方法。默认情况为cv.NORM_L2，适合SIFT和SURF。对于二进制字符串的描述子，如ORB,BRIEF,BRISK等应使用cv.NORM_HAMMING。如果ORB使用WTA_K==3or4，则应使用cv.NORM_HAMMING2。参数crossCheck默认为false。如果为True，则仅返回具有值（i，j）的匹配，使得集合 A 中的第 i 个描述子具有集合 B 中的第 j 个描述子作为最佳匹配，反之亦然。也就是说，两组中的两个特征点应该相互匹配。

### .match()

BFMatcher对象的方法，返回最佳匹配。

.knnMatch()

BFMatcher对象的方法，返回k个最佳匹配，k可以指定。

### 对ORB描述子使用BF

尝试在trainImage中查找queryImage

- 第一步：加载图像，查找描述子

```python
import numpy as np
import cv2 as cv
import matplotlib.pyplot as plt
img1 = cv.imread('box.png',0)          # queryImage
img2 = cv.imread('box_in_scene.png',0) # trainImage
# Initiate ORB detector
orb = cv.ORB_create()
# find the keypoints and descriptors with ORB
kp1, des1 = orb.detectAndCompute(img1,None)
kp2, des2 = orb.detectAndCompute(img2,None)
```

- 第二步：创建一个BF对象并且启用crossCheck获得更好的结果。然后使用.match()方法获得最佳匹配，按照距离升序对它们进行排序，最佳匹配（距离最小）出现在前面，仅画出前十个匹配

```python
# create BFMatcher object
bf = cv.BFMatcher(cv.NORM_HAMMING, crossCheck=True)
# Match descriptors.
matches = bf.match(des1,des2)
# Sort them in the order of their distance.
matches = sorted(matches, key = lambda x:x.distance)
# Draw first 10 matches.
img3 = cv.drawMatches(img1,kp1,img2,kp2,matches[:10], None, flags=2)
plt.imshow(img3),plt.show()
```

效果图：

![matcher_result1.jpg](https://opencv.apachecn.org/docs/4.0.0/img/1b143125b100d1d079dd7625281e4349.jpg)

matches时DMatch对象的列表。具有以下属性

- .distance 描述子之间的距离。越低越好

- .trainldx 目标图像中描述子的索引

- .queryldx 查询图像中描述子的索引

- .imgldx 目标图像的索引
