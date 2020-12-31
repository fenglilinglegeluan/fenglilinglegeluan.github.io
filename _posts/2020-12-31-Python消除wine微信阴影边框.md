[TOC]

# python消除微信阴影边框

#### 1. shebang

**shebang**的作用是指定解释器，这样就可以在终端直接运行程序，或者图形化双击运行程序

有两种书写方式

- `#!/usr/bin/env python3`

  程序执行时会从env环境变量中查找python3的path路径，然后调用解释器。
  
- `#!/usr/bin/python3`

  这个解释器的地址被写死了，调用时直接使用/usr/bin/目录下的python3。

#### 2. os.popen和os.system

这二者都是在python中用来运行系统命令的。

```python
import os
ls = os.popen("ls -l")
print(ls.readlines())
print("\n")
lsresult = os.system("ls -l")
print("start "+str(lsresult))
```

- os.popen() 后台执行命令并返回列表形式的执行结果
- os.system()前台执行命令成功返回0

![image-20201129152807326](image-20201129152807326.png)

#### 3.split()

对字符串进行分片，默认用空格判定

```python
shadowid = item.split()[0]
```

#### 4.linux 命令解析

##### 1.ps -ef | grep WeChat.exe

> -e 所有进程 同 -A
>
> -f 显示全部属性
>
> a 当前终端机下的程序
>
> -H 列出程序间的树状关系

##### 2.wmctrl -l -G -p -x

> -l 在终端中列出当前所有窗口的属性。如果使用了”-p”的参数，属性中将包含窗口的PID值；如果使用了”-G”参数，属性中将包含窗口的几何属性(横竖坐标、宽度和高度)。
>
> -p PID
>
> -G 在通过”-l”动作输出窗口属性时，在输出结果中添加窗口几何属性。
>
> -x 在窗口列表中包含窗口class属性，可以判断是那个进程。

##### 3.xdotool

自动化模拟点击工具

> $xdotool key:模拟按键   例xdotool key ctrl + c
>
> $xdotool type “ “:输出字符串
>
> $xdotool search --name [name of the window] key 在指定的窗口按键
>
> $xdotool mousemove x y click 1 鼠标移动点击 [1：左键，2：滚轮，3：右键]
>
> #!/bin/bash 最后bash脚本执行这个按键操作是最好的选择
>
> $xdotool windowunmap 0x5200015 根据wmctrl列出的窗口号可以用windowunmap进行映射屏蔽。



#### 完整代码

```python
#!/usr/bin/env python3
import os
import time

def exe_existcheck():
    exist = os.popen("ps -ef | grep WeChat.exe")
    exist = exist.readlines() [3]
    #print(exist)
    #print(len(exist))
    if len(exist) < 4:
        print(exist[1],exist[1])
        print("WeChat.exe未启动")
        exit()
    
def shadow_existcheck():
    window = os.popen("wmctrl -l -G -p -x")
    windowid = window.readlines()
    #print(windowid)    
    shadowid = ''
    for item in windowid:
        #print(item+'\n')
        #print(item.find("wechat.exe.Wine"))
        if item.find("wechat.exe.Wine") != 0:
            #print(item+'\n')
            shadowid = item.split()[1]
            break
    window.close()
    return shadowid
def xdotool_check(shadowid):
    if shadowid != '':
        shadowid = shadowid[:-3] + "0015"
        print(shadowid) 
        os.system("xdotool windowunmap " + shadowid)
    else:
        print("WeChat not display yet.")

if __name__ == '__main__':
    while True:
        time.sleep(6)
        exe_existcheck()
        shadowid = shadow_existcheck()
        xdotool_check(shadowid)
```

