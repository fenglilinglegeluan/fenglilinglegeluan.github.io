---
layout: mypost
title: 拷贝应用目录里的文件,以及 andrax 访问 sdcard 中内容
categories: [android, andrax]
---
### 1. 利用 adb 进入 shell


```bash
adb shell
```

### 2. 逐级给权限


```bash
chmod 777 /data     
chmod 777 /data/data/
chmod 777 /data/data/packname/databases/db_file.db
```

### 3. 拷出文件


```bash
adb pull /data/data/packname/databases/db_file.db #push 是拷入文件
```

### 图

1、2 步骤需要 `adb devices` 能看到设备才能连上 `shell`

![image-20220503171153984.png](image-20220503171153984.png)

3、步骤 需要退出 `shell`

![image-20220503171344838.png](image-20220503171344838.png)

> 冲动：手机中装了 andrax 充当 移动 linux 电脑，但是不能访问手机 sdcard 中的文件，不方便拷东西。

使用 abd 进 shell ，输入以下命令挂在到 andrax 根目录里就好了

```bash
mount -o bind /mnt/sdcard/ /data/data/com.thecrackertechnology.andrax/tmpsystem/sdcard/
```

