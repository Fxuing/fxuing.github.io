---
title: 阿里开源项目——EasyExcel使用
date: 2020年6月19日
comments: true
categories: java
tags:
- Excel
---

# EasyExcel使用

EasyExcel是一个基于Java的简单、省内存的读写Excel的开源项目。在尽可能节约内存的情况下支持读写百M的Excel。

<!-- more -->

官方文档写的非常详细：[EasyExcel · 语雀](https://www.yuque.com/easyexcel/doc/easyexcel)

GitHub：https://github.com/alibaba/easyexcel

---

## 指定列动态导出

在导出的基础上，添加导出字段，可实现动态导出数据



根据官方文档，有两种导出方式，一种需要手动关闭文件流，一种自动关闭文件流，在此我选择自动关闭文件流的方式。

```java
    ExcelWriterBuilder excelBuilder = EasyExcel.write(response.getOutputStream(), ExcelUserDTO.class);
    excelBuilder = includeColumnFiledName(includeColumnFiledNames, excelBuilder);
    excelBuilder.sheet("sheet").doWrite(data());
```

指定列动态导出关键代码：

```java

    private ExcelWriterBuilder includeColumnFiledName(Set<String> includeColumnFiledNames, ExcelWriterBuilder excelBuilder) {
        excelBuilder = excelBuilder.registerWriteHandler(new LongestMatchColumnWidthStyleStrategy());
        if (includeColumnFiledNames.size() > 0) {
            excelBuilder = excelBuilder.includeColumnFiledNames(includeColumnFiledNames);
        }
        excelBuilder = excelBuilder.registerWriteHandler(new LongestMatchColumnWidthStyleStrategy());
        return excelBuilder;
    }
```

## 复杂表头制作

EasyExcel复杂表制作也非常简单，使用`@ExcelProperty`注解