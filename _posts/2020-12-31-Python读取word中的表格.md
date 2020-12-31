---
layout: mypost
title: Python读取word中的表格
categories: [Python,文本处理]
---

## word读取操作

- ##### 导入python-docx

  ```python 
  from docx import Document
  import os #用于获取文件路径
  ```

- ##### 读取word
	
	```python
	docx_files = []
	for root,dirs,files in os.walk("/home/ltt/2019/")#遍历目录，由浅到深
		for file in files:
        docx_files.append(os.path.join(root,file))#创造一个每个文件是绝对路径文件名的列表
	#os.path.join() 有一系列的判断，判断如何拼接文件名等。
	#root 绝对路径
	#dirs 当前文件夹名
	#files 当前文件夹中文件名
	```
	
	 ![image-20201207110221829](image-20201207110221829.png)
	
	
	```python
	document = readdoc("/home/ltt/2019/1.docx")
	```
	
- ##### word中的表操作

	```python
	tables = document.tables #读取word中的所有表
	table = tables[0] #对tables中的第一个表进行操作
	len(table.rows) #行数
	len(table.columns) #列数
	```

- ##### 读取表中数据

	```python
	content = table.rows[0] #读取第1行的数据
	content.cells[0].text #输出第1行第1列的数据
	同理可以先使用table.columns[0],再使用content.cells[0].text填写行号输出单元格内容。
	```
	
	第二种方法
	
	```python
	table.cell(0,0).text #读取1行1列的数据
	```
	
	