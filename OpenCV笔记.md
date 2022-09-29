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

一般通过.shape访问图像形状。

### .dtype

一般通过.dtype来访问图像数据类型，代码中的大量错误是由无效的数据类型引起的。

## 图像中的感兴趣区域

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

### 制作图像边界（填充）

### cv.copyMakeBorder()

该函数采用以下参数：、

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

![](https://opencv.apachecn.org/docs/4.0.0/img/border.jpg)

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




