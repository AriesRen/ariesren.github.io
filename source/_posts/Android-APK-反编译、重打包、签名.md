---
title: Android APK 反编译、重打包、签名
date: 2019-01-10 15:39:34
tags: ['反编译', 'apktool']
categories: 
---


首先声明，反编译别人的APK是一件不厚道的事情，本文抱着学习的态度，学习如何使用工具反编译Android APK。


### 一、工具

[apktool](https://ibotpeaches.github.io/Apktool/), 编译和反编译apk，提取图片、布局等资源
[dex2jar](https://github.com/pxb1988/dex2jar/releases)，将可运行文件class.dex反编译为可读的jar源代码
[jd-gui](http://jd.benow.ca), 查看jar源代码

### 二、反编译

#### 2.1、 apktool安装

**windows下安装**

1、安装java，配置环境变量
2、下载最新[apktool.bat](https://raw.githubusercontent.com/iBotPeaches/Apktool/master/scripts/windows/apktool.bat)
3、下载最新[apktool.jar](https://bitbucket.org/iBotPeaches/apktool/downloads/)
4、将apktool.bat和apktool.jar放在同一目录下，然后就可以在命令行使用了

##### 2.2、 用法

可以直接在命令行执行apktool.bat使用。这里介绍两个最常用的用法。

**反编译**

```shell
apltool.bat d -o <output_dir> test.apk
```
其中<output_dir>指定输出目录，默认为apk名称，test.apk为需要反编译的apk

**编译**

```shell
apltool.bat b -o <output.apk> <input_dir>
```
其中<input_dir>为上面反编译输出的目录，<output.apk> 是编译输出的apk

apktool反编译后的典型目录如下：

{% asset_img 1.jpg %}

此时，可以查看AndroidMainfest.xml、res、smali文件了。甚至可以修改目录下的资源文件或smali文件，然后重新编译打包发布。值得注意的是，apktool反编译出来的是smali文件，即汇编语言版本，并不能查看源代码。

### 三、查看源代码

查看源代码的话，这里需要用到dex2jar、jd-gui这两个工具。

1、将需要反编译的apk后缀改为rar或zip并解压到一个文件夹，得到其中的class.dex
2、用dex2jar工具反编译class.dex得到jar文件 classes-dex2jar.jar

{% asset_img 2.jpg %}

3、使用jd-gui打开classes-dex2jar.jar，就可以查看源码了，当然如果代码发布的时候做过混淆，我们也只能看到混淆过的代码。所以代码混淆的重要性不言而喻。

{% asset_img 3.jpg %}

### 四、修改代码

如果只是修改APK相应的资源，例如图片、字符串比较好办，在res文件夹找到相应的文件替换即可。

修改代码就比较麻烦，因为反编译出来的结果只有smali文件，即java虚拟机支持的汇编语言。如果确实需要修改代码，就得对照smali文件和反编译出来的源码，按照smali规范来改代码，相当于写汇编，所以难度还是比较大的。

### 五、重新打包

修改代码完成后，还需要重新打包以及重新签名。重新打包使用apktool编译修改过的目录即可。

```shell
apltool.bat b -o <output.apk> <input_dir>
```

### 六、签名

签名是要对发布的apk文件做标记，确保你的apk文件有唯一的身份归属认证，只有相同的签名和相同包名的文件才可以覆盖安装并保留用户信息。

对于反编译的apk，我们可以用java工具jarsigner来对它进行签名。

1、生成keystroe文件

签名需要keystore文件，可以使用keytool工具生成，java环境一般都自带keytool命令，可以直接在命令行中进行测试。

```shell
keytool -genkey -alias demo.keystore -keyalg RSA -VALIDITY 40000 -keystore demo.keystore
```

各个参数意义如下：
-genkey 产生证书文件
-alias 产生别名
-keystore 制定密钥库的.keystore文件
-keyalg 制定密钥算法 这里指定为RSA 非对称密钥算法
-validity 证书有效天数

{% asset_img 4.jpg %}

2、签名apk

签名工具使用jarsigner，jarsigner也存在于Java JDK中，所以如果安装好Java环境，可以直接在命令行中使用。

```shell
jarsigner -verbos -keystore <keystore密钥库位置> <待签名的APK>  <密钥库别名>
```

-verbose 制定生成详细输出
-keystore 制定数字证书存储路径

这样就完成了对APK的重签名过程，然后就可以安装使用了。如果你的手机上原来就有这个APP，则需要卸载之后重新安装，因为签名已经改变。




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