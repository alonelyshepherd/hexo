---
title: 使用Apache POI类库操作Excel
date: 2016-04-24 18:56:29
tags:
---
### 背景
最近一个朋友找我咨询Excel方面的问题，为了解答他的问题，我查阅了Excel的函数库，然后就感觉Excel里的预置函数更多是为不懂得编程的人设计的，比较傻瓜化，带来的后果就是不够灵活：明明一个很简单的功能如果使用预置公式的话就要绕很大一个圈子才能实现。所以我觉得有必要自己研究一下。
> PS:本文会不定期更新，我会把后续总结的Excel操作放上来

#### 概念
首先明确POI中的几个概念：

 - WorkBook：代表你要操作的Excel文件
 - Sheet：Excel中的工作簿
 - Row：行
 - Column：列，注意，Excel是基于行的，**POI中其实没有Column这个概念**，我写出来只是为了强调。
##### 首先创建一个WorkBook
POI针对Office 2007之前和之后的版本提供了两个Excel的表示，分别是：`HSSFWorkbook`和`XSSFWorkbook`。

- `HSSWorkbook` Office 2007之前的版本使用这个表示
- `XSSFWorkbook` Office 2007以及之后的版本使用这个表示

具体创建一个Excel的WorkBook表示方法如下：

    	HSSFWorkbook xlsWorkbook = new HSSFWorkbook(new FileInputStream(xlsWorkbookPath));
		XSSFWorkbook xlsxWorkbook = new XSSFWorkbook(new FileInputStream(xlsxWorkbookPath));
		
这样是不是很麻烦？别担心POI提供了工厂方法：

        Workbook srcWorkBook = WorkbookFactory.create(new FileInputStream(srcPath));
    
好了，上面我们创建了Workbook，下面我们来看看如何操作Workbook。
##### 获取工作簿
    Sheet sheet0 = srcWorkBook.getSheetAt(0);
##### 获取某一行
    Row row = sheet0.getRow(0);
##### 获取最大行数的方法
    int maxRows = sheet.getPhysicalNumberOfRows();
##### 获取某个单元格的方法
    Cell cell0 = row.getCell(0);
##### 获取单元格内容的方法
    cell.getStringCellValue(); // 文本类型的获取方法
POI中单元格的值一共有六种：

 - 空
 - 布尔型
 - 错误
 - 公式
 - 数值
 - 文本
 大部分单元格的值都属于数值型，不同单元格获取值的方法也不同。
##### 设置单元格内容的方法
    cell.setCellValue(value)
##### 保存的方法

    FileOutputStream fos = new FileOutputStream(destPath);
	destWorkBook.write(fos);
	fos.close();
[Apache POI官网的地址][1]
> 待续:)

  [1]: http://poi.apache.org