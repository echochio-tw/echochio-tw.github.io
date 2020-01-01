---
layout: post
title: widnwos 視窗異動比對(python)
date: 2020-01-01
tags: windows python
---
抄抄改改 ....

連續通知的不發告警

小數的變成 1 兩次的發告警 .....

```
from ctypes import *
from ctypes.wintypes import *
import cv2
from win32 import win32gui
import win32con
import win32ui
import win32api
import re
from time import sleep
from skimage.measure import compare_ssim

def get_current_size(hWnd):
    try:
        f = ctypes.windll.dwmapi.DwmGetWindowAttribute
    except WindowsError:
        f = None
    if f:
        rect = ctypes.wintypes.RECT()
        DWMWA_EXTENDED_FRAME_BOUNDS = 9
        f(ctypes.wintypes.HWND(hWnd),
          ctypes.wintypes.DWORD(DWMWA_EXTENDED_FRAME_BOUNDS),
          ctypes.byref(rect),
          ctypes.sizeof(rect)
          )
        size = (rect.right - rect.left, rect.bottom - rect.top)        
        return size

def FindWindow_bySearch(pattern):
    window_list = []
    win32gui.EnumWindows(lambda hWnd, param: param.append(hWnd), window_list)
    for each in window_list:
        if re.search(pattern, win32gui.GetWindowText(each)) is not None:
            return each
            
def get_pic(pic_data):
    # 获取桌面
    hdesktop = win32gui.GetDesktopWindow()

    hwnd = FindWindow_bySearch("RiskControl_Air")
    left, top, width, height = win32gui.GetWindowRect(hwnd)
    width, height = get_current_size(hwnd)

    # 创建设备描述表
    desktop_dc = win32gui.GetWindowDC(hdesktop)
    img_dc = win32ui.CreateDCFromHandle(desktop_dc)

    # 创建一个内存设备描述表
    mem_dc = img_dc.CreateCompatibleDC()

    # 创建位图对象
    screenshot = win32ui.CreateBitmap()
    screenshot.CreateCompatibleBitmap(img_dc, width, height)
    mem_dc.SelectObject(screenshot)

    # 截图至内存设备描述表
    mem_dc.BitBlt((0, 0), (width, height), img_dc, (left, top), win32con.SRCCOPY)

    # 将截图保存到文件中
    screenshot.SaveBitmapFile(mem_dc, pic_data)

    # 内存释放
    mem_dc.DeleteDC()
    win32gui.DeleteObject(screenshot.GetHandle())

last1 = 1.0
last2 = 1.0
while True:
    get_pic("D:/tmp/check1.bmp")
    sleep(5)
    get_pic("D:/tmp/check2.bmp")
    
    imageA = cv2.imread("D:/tmp/check1.bmp")
    imageB = cv2.imread("D:/tmp/check2.bmp")
    grayA = cv2.cvtColor(imageA,cv2.COLOR_BGR2GRAY)
    grayB = cv2.cvtColor(imageB,cv2.COLOR_BGR2GRAY)
    (score,diff) = compare_ssim(grayA,grayB,full = True)
    diff = (diff *255).astype("uint8")
    if (last2 < 1.0):
        if (score == 1.0) and (last1 == 1.0):
            print ("剛剛有變化")
    print("SSIM:{}".format(score))
    last2 = last1 
    last1 = score
```
