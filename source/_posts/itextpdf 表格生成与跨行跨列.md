---
title: itextpdf 表格生成与跨行跨列
date: 2020年6月24日
comments: true
categories: java
tags:
- itextpdf
---


## itextpdf 表格生成与跨行跨列


由于前段时间需要做需求接触pdf表格，表格需要跨行跨列操作，写了个工具类，代码如下：

```java
static class PdfUtil {

        /**

         * 生成一个表格

         * @author hou_fx

         * @param total  总列数

         * @param textFont 字体

         * @param data   表格数据     X行    Y列

         * @param doc    PDF文档对象

         * @throws DocumentException

         */

        public static void TableBule(int total,Font textFont,List> data,Document doc) throws DocumentException{

            // 创建一个有N列的表格

            PdfPTable table = new PdfPTable(total);

            table.setPaddingTop(20);

            table.setSpacingAfter(20);

            table.setTotalWidth(530); //设置列宽

            // table.setTotalWidth(new float[]{ 100, 165, 100, 165 }); //设置列宽

            table.setLockedWidth(true); //锁定列宽

            PdfPCell cell;

            for (int i = 0; i < data.size(); i++) {  //遍历数据行   每个数据行都是一个list

                Iterator it = data.get(i).iterator();

                int count=0;

                while (it.hasNext()) {               //遍历每行数据，每个数据都是一个单元格

                    cell = new PdfPCell(new Phrase(it.next(), textFont));

                    cell.setMinimumHeight(17); //设置单元格高度

                    cell.setUseAscender(true); //设置可以居中

                    //第一个单元格背景色

                    if (count%2==0) {

                        cell.setBackgroundColor(new BaseColor(231,230,230));

                    }

                    cell.setHorizontalAlignment(Element.ALIGN_LEFT); //左对齐

                    cell.setVerticalAlignment(Element.ALIGN_MIDDLE); //设置垂直居中

                    table.addCell(cell);

                    count++;

                }

            }

            doc.add(table);

        }
        /**
         * 生成一个表格
         * @author hou_fx
         * @param total 总列数
         * @param textFont 字体
         * @param data  表格数据     X行    Y列
         * @param doc PDF文档对象
         * @param colspan 第几列
         * @param rowspan 第几行
         * @param number  跨几列
         * @throws DocumentException
         */
        public static void TableColspan(int total,Font textFont,List<List<String>> data,Document doc,int[] rowspan,int[] colspan,int[] number) throws DocumentException{
            // 创建一个有N列的表格
            PdfPTable table = new PdfPTable(total);
            table.setPaddingTop(20);
            table.setSpacingAfter(20);
            table.setTotalWidth(530); //设置列宽
            // table.setTotalWidth(new float[]{ 100, 165, 100, 165 }); //设置列宽
            table.setLockedWidth(true); //锁定列宽
            PdfPCell cell;
            //数组下标
            int cos=0;
            for (int i = 0; i < data.size(); i++) {  //遍历数据行   每个数据行都是一个list
                Iterator<String> it = data.get(i).iterator();
                int count=0;
                while (it.hasNext()) {               //遍历每行数据，每个数据都是一个单元格
                    cell = new PdfPCell(new Phrase(it.next(), textFont));
                    cell.setMinimumHeight(17); //设置单元格高度
                    cell.setUseAscender(true); //设置可以居中
                    if (cos<rowspan.length && i==rowspan[cos]-1 && count==colspan[cos]-1) {
//                    if (i==rowspan[cos]-1) {//行
//                        if (count==colspan[cos]-1) {//列
                        cell.setColspan(number[cos]);//跨单元格
                        cos++;
//                        }
//                    }
                    }
                    cell.setHorizontalAlignment(Element.ALIGN_LEFT);
                    cell.setVerticalAlignment(Element.ALIGN_MIDDLE); //设置垂直居中
                    table.addCell(cell);
                    count++;
                }
            }
            doc.add(table);
        }
    }
```

部分代码，可根据实际需求进行扩展。
效果图如下：
![](https://img-blog.csdnimg.cn/20190215163240830.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTQ5Mjc1,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20190215163345845.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTQ5Mjc1,size_16,color_FFFFFF,t_70)![](https://img-blog.csdnimg.cn/20190215163423489.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTQ5Mjc1,size_16,color_FFFFFF,t_70)
