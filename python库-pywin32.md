# 鼠标操作

## 移动光标

SetCursorPos(x,y)

## 综合鼠标击键和鼠标动作

mouse_event(win32con.flag,0,0,0,0)

flag:

MOUSEEVENTF_MOVE:表明发生移动。

MOUSEEVENTF_LEFTDOWN:表明接按下鼠标左键。

MOUSEEVENTF_LEFTUP:表明松开鼠标左键。

MOUSEEVENTF_RIGHTDOWN:表明按下鼠标右键。

MOUSEEVENTF_RIGHTUP:表明松开鼠标右键。

MOUSEEVENTF_MIDDLEDOWN:表明按下鼠标中键。

MOUSEEVENTF_MIDDLEUP:表明松开鼠标中键。

