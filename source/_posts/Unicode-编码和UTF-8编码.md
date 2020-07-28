---
title: Unicode 编码和UTF-8编码
date: 2020-07-28 20:40:02
tags:
categories:
---

## ASCII

在早起计算机中，字符是用ASCII码来表示的。ASCII码表中只有26个字母和一些其他的特殊字符和符号。下表提供了ASCII字符及其对应的十进制和十六进制的值。

{% asset_img ascii-codes.gif %}

从上表可以推断，在十进制数字系统中，ASCII值可以从0到127表示，让我们看一下8位字节中0和127的二进制表示形式。

0表示为

{% asset_img 0---binary.png %}

127表示为

{% asset_img 127-binary.png %}

从上面的二进制表示可以推断出，ASCII只使用了7个byte来表示0-127，但是第8位没有使用，这就使得各个编码有了不同的地方。

我们知道，如果使用第8位的话，可以表示十进制128-255，而各个国家使用128-255表示的字符不同，例如越南人使用十进制值182表示越南字母ờ，而印度使用相同的十进制值182表示印地语字母घ。因此，如果印度人写的电子邮件中包含字母घ，越南人阅读该电子邮件时，则该字母显示为ờ。这显然不是我们希望的。

## Unicode and Code Points

Unicode 字符集将世界上的每个字符都映射了一个唯一的数字，这确保了不同的字母之间不会发生冲突，并且这些数字是与平台无关的。

这些唯一的数字在unicode中被称为code point。让我们看一下在Unicode中是怎样表示的。

例如在latin字符中ṍ 的code point为`U+1E4D`。**U**表示Unicode，**1E4D**是分配给ṍ 的十六进制值。

英文字母A的code point表示为`U+0041`。

如果想了解更多语言和字母的code point，参考 http://www.unicode.org/charts/

## UTF-8编码

既然我们知道了什么是Unicode，以及如何将世界上的每个字母分配给一个唯一的code point，我们需要一种在计算机内存中计算表示这些code point的方法。UTF-8就是其中的一种方法。

**UTF-8编码是一种可变大小的编码方案，用于表示内存中unicode的code point。可变大小的编码表示根据其大小使用1、2、3、4个字节表示code point**


### UTF-8 1 byte encoding

UTF-8 1字节编码是用第一个比特位 置0 来标识。

{% asset_img utf-8-1-byte-encoding.png %}

比如英文字母A用unicode code point表示为 `U+0041`。它的二进制表示形式为`1000001`。

所以A用UTF-8 1字节编码表示为

{% asset_img A-unicode-utf-8-1.png %}

红色的O位表示已使用1字节编码，其余位表示代码点

### UTF-8 2 byte encoding 

拉丁字母ñ 的code point为 `U+00F1`,用二进制表示为11110001，该值大于使用1字节编码可以表示的最大值，因此此字母将使用UTF-8 2字节编码表示。

通过在第一个bit设置110 和 第二个bit设置10 来标识2字节编码。

{% asset_img UTF-8-2-byte-encoding.png %}



<br>
<br>
<br>

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- 信息流广告 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4127326375481893"
     data-ad-slot="9105526840"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>