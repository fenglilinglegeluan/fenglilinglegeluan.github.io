---
layout: mypost
title: Python-excel学习
categories: [Python,文本处理]
---

[TOC]

## 调用库文件

```python
import xlrd #读
import xlwt #写
#调用读写excel功能
```

## 读

##### 读取文件

```python
excel = xlrd.open("./data.xls") #读取excel文件
sheetname = excel.sheet_name() #读表名
print(sheetname) #打印所有表名
```

##### 选择表

```python
sheet = excel.sheet_by_index(0) #按索引选择表
或者
sheet = excel.sheet_by_name('sheet1') #按名字选择表
```

##### 读表

```python
tname = sheet.name #读表名
nrows = sheet.nrows #读行数
ncols = sheet.ncols #读列数
```

##### 读表中数据

```python
temp = sheet.col_values(int) #读取第int列数据
temp = sheet.row_values(int) #读取第int行数据
teachername = list(set(temp)) #去重
```

## 写

##### 创建excel文件

```python
xls = xlwt.Workbook(encoding='utf-8',style_compression=0) #在内存中创建一个excel文件，style_compression说明了是否允许改变excel表格样式。
```

##### 创建表

```python
sheet = xls.add_sheet('sheet0',cell_overwrite_ok=False) #创建一sheet0表，默认单元格操作时如果单元格有值不允许覆盖。
```

##### 写表中数据

```python
sheet.write(0,0,data) #在（0,0）（前行，后列）单元格中写数据
sheet.write_merge(0,0,0,15,data) #在单元格中（0,0）写数据并且合并第一行的0-15列
```

## 保存

```python
xls.save('/path'+str+'.xls') #保存excel到硬盘本地
```