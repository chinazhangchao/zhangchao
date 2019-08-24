---
title: "扫描版PDF文档转文本方法"
date: 2019-08-24T18:44:59+08:00
type: posts
categories: [工具]
keywords: [pdf,ocr]
tags: [工具]
---

很多pdf文档是扫描版的，也就是图片，无法提取文字，使用很不方便。通过结合以下两个利器可以很方便的把pdf转为文本。
<!--more-->
#### 1. 把pdf文档转为多张图片
可以通过XpdfReader工具把pdf文档转为一组图片。
假设要把`1.pdf`转为一组`jpg`图片，放到`test`目录下。可以使用以下命令：
```shell
pdfimages -j 1.pdf test
```
下载地址：https://www.xpdfreader.com/download.html

#### 2. 通过OCR识别图片中的文字
使用谷歌的OCR识别工具tesseract把文字提取出来。
把图片`1.jpg`转为文字保存在`1.txt`中，可以使用以下命令：
```shell
tesseract 1.jpg 1.txt -l chi_sim
```
后面的参数` -l chi_sim`表示要转换的文字是中文。

如果想一次转换很多张图片，可以把要转换的图片文件路径写入到一个文本文件中，比如`in.txt`：
```shell
1.jpg
2.jpg
3.jpg
4.jpg
5.jpg
6.jpg
```
使用以下命令一次性全部转换保存到`out.txt`中：
```shell
tesseract in.txt out.txt -l chi_sim
```
下载地址：https://github.com/tesseract-ocr/tesseract

搞定，收工，如有疑问或建议欢迎留言讨论。
