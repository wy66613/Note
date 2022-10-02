# 子包

- pynput.mouse：包含控制和监控鼠标或触摸板的类

- pynput.keyboard：包含控制和监控键盘的类

# 鼠标模块

## 鼠标基本操作

### 获取鼠标操控对象

control=mouse.Controller()

### 获取鼠标当前位置

print(control.positon)

### 改变鼠标位置

control.position=(x,y)

### 移动鼠标位置

control.move(x,y)

### 按下鼠标对应键

control.press(flag) 单击

control.click(flag,2) 双击

### 鼠标按键类型

- 左键 mouse.Button.left

- 右键 mouse.Button.right

- 中键 mouse.Button.middle

### 鼠标滚轮向上或向下滚动

control.scroll(0,+/-100) +为上，-为下

## 鼠标事件监听

```python
from pynput import mouse

# 鼠标move监听
def on_move(x, y):
    print(f'Current position： ({x}, {y})')


# 鼠标click监听
def on_click(x, y, button, pressed):
    print(f'Click position： ({x}, {y})')
    print(f'Click button： {button}')
    print(f'Click state: {"Pressed" if pressed else "Release"}')


# 鼠标滚轮scroll监听
def on_scroll(x, y, dx, dy):
    print(f'Scroll position： ({x}, {y})')
    print(f'Scroll direction： ({dx}, {dy})')


with mouse.Listener(on_move=on_move, on_click=on_click, on_scroll=on_scroll) as listener:
    listener.join()
```

# 键盘模块

## 键盘模拟控制

### 获取按键

使用keyboard.KeyCode.from_vk通过键盘的映射码来获取

| 键     | 键码                              |
| ----- | ------------------------------- |
| a     | 65                              |
| b     | 66                              |
| c     | 67                              |
| d     | 68                              |
| e     | 69                              |
| f     | 70                              |
| g     | 71                              |
| h     | 72                              |
| i     | 73                              |
| j     | 74                              |
| k     | 75                              |
| l     | 76                              |
| m     | 77                              |
| n     | 78                              |
| o     | 79                              |
| p     | 80                              |
| q     | 81                              |
| r     | 82                              |
| s     | 83                              |
| t     | 84                              |
| u     | 85                              |
| v     | 86                              |
| 0     | 96                              |
| 1     | 97                              |
| 2     | 98                              |
| 3     | 99                              |
| 4     | 100                             |
| 5     | 101                             |
| 6     | 102                             |
| 7     | 103                             |
| 8     | 104                             |
| 9     | 105                             |
| *     | 106                             |
| +     | 107                             |
| Enter | 108                             |
| -     | 109                             |
| ·     | 110                             |
| /     | 111 |
| 其它等等  |                                 |

### 获取键盘操作对象

control=keyboard.Controller()

### 模拟按键操作

- 按下 a 键

control.press(keyboard.KeyCode.from_char('a'))

control.press('a')

- 释放 a 键

control.release(keyboard.KeyCode.from_char('a'))

control.release('a')

- 同时按下 shift 和 a

```python
from pynput import keyboard

control = keyboard.Controller()
with control.pressed(keyboard.Key.shift):
    control.press('a')
    control.release('a')
```

- 输出字符串

control.type(string)

## 键盘事件监听

```python
from pynput import keyboard

def on_press(key):
    """按下按键时执行。"""
    try:
        print('alphanumeric key {0} pressed'.format(
            key.char))
    except AttributeError:
        print('special key {0} pressed'.format(
            key))
    #通过属性判断按键类型。

def on_release(key):
    """松开按键时执行。"""
    print('{0} released'.format(
        key))
    if key == keyboard.Key.esc:
        # Stop listener
        return False

# Collect events until released
with keyboard.Listener(
        on_press=on_press,
        on_release=on_release) as listener:
    listener.join()
```

注：

- 当两个函数中任意一个返回False（还有就是释放Exception或继承自Exception的异常）时，就会结束进程。

- 可以用listener.start()和listener.stop()代替with语句。
