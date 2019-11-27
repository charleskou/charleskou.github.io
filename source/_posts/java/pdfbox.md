---
title: PDFBox使用指南
tags: [Java]
date: 2019-10-30 17:11:33
---


## 1. 介绍
Apache PDFBox是一个开源Java库，支持PDF文档的开发和转换。使用该库可以开发创建，转换和操作PDF文档的Java程序。

## 2. 特性
- 提取文本 - 使用PDFBox，您可以从PDF文件中提取Unicode文本。
- 拆分和合并 - 使用PDFBox，您可以将单个PDF文件分成多个文件，并将它们作为单个文件合并。
- 填写表单 - 使用PDFBox，您可以填写文档中的表单数据。
- 打印 - 使用PDFBox，您可以使用标准Java打印API打印PDF文件。
- 另存为图像 - 使用PDFBox，您可以将PDF保存为图像文件，如PNG或JPEG。
- 创建PDF - 使用PDFBox，您可以通过创建Java程序创建新的PDF文件，还可以包括图像和字体。
- 签名 - 使用PDFBox，您可以将数字签名添加到PDF文件。

## 3. 引入
```
<dependency>
  <groupId>org.apache.pdfbox</groupId>
  <artifactId>pdfbox</artifactId>
  <version>2.0.17</version>
</dependency>
```

## 4. 应用示例

### 4.1 PDFBox指定位置添加文本
指定字体文件simsun.ttf支持添加中文文本
```java
PDDocument doc = null;
try {
    File file = new File("C:\\origin.pdf");
    doc = PDDocument.load(file);

    String fontFilePath = "C:\\simsun.ttf";
    PDType0Font font = PDType0Font.load(doc, new File(fontFilePath));

    PDPage page = doc.getPage(0);
    PDPageContentStream contentStream = new PDPageContentStream(doc, page, PDPageContentStream.AppendMode.PREPEND,
            true);
    contentStream.beginText();
    contentStream.setFont(font, 10);
    float offsetx = 40;
    float offsety = 105;
    contentStream.newLineAtOffset(offsetx, offsety);
    contentStream.showText("hello, world");
    contentStream.endText();
    contentStream.close();

    doc.save("C:\\result.pdf");

    doc.close();
} catch (IOException e) {
    // TODO handle exception
}
```


### 4.2 PDFBox加载ttc字体文件
以宋体 - SimSun为例
```java
PDType0Font font;
TrueTypeCollection trueTypeCollection = new TrueTypeCollection(new File("c:/windows/fonts/simsun.ttc"));
TrueTypeFont trueTypeFont = trueTypeCollection.getFontByName("SimSun");
if (null != trueTypeFont) {
    font = PDType0Font.load(doc, trueTypeFont, false);
}
trueTypeCollection.close();
```

### 4.3 添加注释Annotation
示例代码文件：https://github.com/apache/pdfbox/blob/2.0.17/examples/src/main/java/org/apache/pdfbox/examples/pdmodel/AddAnnotations.java



## 5. 参考
- 官网 https://pdfbox.apache.org/
- Github Mirror https://github.com/apache/pdfbox
- PDFBox快速指南 http://www.vue5.com/pdfbox/pdfbox_quick_guide.html


## 6. 使用
执行一次的任务：
- JDK / JRE
- pdf.zip
- C盘创建pdf文件夹，下载相应文件

可重复执行的任务：
- 1. 将PDF复制到C盘pdf文件夹下，同时在此路径下使用sublime软件创建PDF同名文本文档(后缀为.txt)
- 2. 在资源管理器地址栏中输入命令：cmd，打开命令行窗口
- 3. 在命令行窗口中输入：java -jar de[Tab按键补全]
- 4. 浏览器访问 http://localhost:8080/pdf
- 5. 查看结果文件，存储在C盘pdf/result文件夹中