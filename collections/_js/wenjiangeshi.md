---
layout: post
title: MIME, Base64, Blob
---
## MIME
"Multipurpose Internet Mail Extensions"，中译为"多用途互联网邮件扩展"，指的是一系列的电子邮件技术规范。
早期的邮件只能只能使用ASCII字符，不能发图片和附件。

所以用`Content-Type`来标记文件格式。它包含了主要类型（primary type）和次要类型（subtype）两个部分，两者之间用"/"分割。主要类型有9种，分别是application、audio、example、image、message、model、multipart、text、video。

如果信息的主要类型是"text"，那么还必须指明编码类型"charset"，缺省值是ASCII，其他可能值有"ISO-8859-1"、"UTF-8"、"GB2312"等等。

```
text/plain; charset="ISO-8859-1"：纯文本，文件扩展名.txt
text/html：HTML文本，文件扩展名.htm和.html
image/jpeg：jpeg格式的图片，文件扩展名.jpg
image/gif：GIF格式的图片，文件扩展名.gif
audio/x-wave：WAVE格式的音频，文件扩展名.wav
audio/mpeg：MP3格式的音频，文件扩展名.mp3
video/mpeg：MPEG格式的视频，文件扩展名.mpg
application/zip：PK-ZIP格式的压缩文件，文件扩展名.zip
```

MIME用`Content-transfer-encoding`将8位的非英语字符转化为7位的ASCII字符。
可选值有"7bit"、"8bit"、"binary"、"quoted-printable"和"base64"----其中"7bit"是缺省值，即不用转化的ASCII字符。
真正常用是"quoted-printable"和"base64"两种。

## base64
**好处**
1. 所有的二进制文件，都可以因此转化为可打印的文本编码，使用文本软件进行编辑；
2. 能够对文本进行简单的加密。

### Quoted-printable
用于ACSII文本中夹杂少量非ASCII码字符的情况，不适合于转换纯二进制文件，将每一个8位的字节，转换为3个字符。
