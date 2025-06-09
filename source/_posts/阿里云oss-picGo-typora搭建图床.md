---
title: 阿里云oss-picGo-typora搭建图床
date: 2025-06-09 01:10:10
tags:
  - Hexo
  - Markdown
  - 阿里云OSS
  - picGo
  - typora
categories: 问题记录
---

## Typora 图床设置

前言：因为开始记录博客，发现本地存储的图片路径与服务器路径总是不太一样，总是复制粘贴又不太方便，在此选择搭建图床来进行使用。

## 软件介绍

我们需要将图片上传到云（专业称图床）上，而云有多种，如下：

- 阿里云oss 专业，快速，存储空间便宜，一年9块钱40G。但是下行流量需要另外收费。

- github 免费。但不是专业图床，国内访问速度太慢。

- gitee 免费，快速。但不是专业图床，有防盗链风险，比如微信浏览器就打不开gitee的图，gitee官方是禁止用来做图床的。


此处使用阿里云oss 因为网上相关文档比较多，介绍的详细，虽然收费，但是比较稳定，快速，价格也便宜。

而有了选择好图床之后，那就需要上传图片的工具，而大家常用的工具为：PicGo ，PicGo是一个用于快速上传图片并获取图片URL链接的工具，目前支持微博图床、七牛图床、腾讯云、又拍云、GitHub、阿里云、Imgur等图床。

## 购买对象存储oss

点击产品，存储，对象存储oss。

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-1.png)

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-3.png)

大部分配置都是为默认，购买时长自己选择，这边选择1年，那就是9元。点击支付即可。

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-2.png)

## 阿里云OSS存储

注册或登录阿里云服务 [阿里云-计算，为了无法计算的价值](https://www.aliyun.com/)

打开控制台

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-4.png)

然后回到对象存储oss界面点击管理控制台（没有开通时，显示的是立即开通，开通之后变成了管理控制台）

打开oss控制台

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-5.png)

如果没有开通，则显示

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-6.png)

这样，请选择立即开通（免费）

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-7.png)

点击管理控制台后，找到Buket进行创建

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-8.png)

输入Bucket名称，选择离自己最近的地域，【记住名称和所选地域后面使用】读写权限一定要写公共读！！！这是重点！！！选好之后点击确定。

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-9.png)

创建成功显示：

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-10.png)

创建成功后，再点击头像，选择ACCessKey管理

 ![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-11.png)

选择使用子用户使用AccessKey（为了安全）

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-12.png)

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-13.png)

记录自己的

| AccessKey ID | AccessKey Secret     |
| ------------ | -------------------- |
| xxxxxxxxxxxx | xxxxxxxxxxxxxxxxxxxx |


创建用户权限

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-14.png)

到目前为止，我们阿里云oss中已完成全部操作。

## PicGo 软件安装操作

下载picgo软件
首先进入此网址： https://github.com/Molunerfinn/PicGo/releases
进入之后，往下翻找到软件安装包，

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-15.png)

下载完成之后，放置桌面，然后双击运行安装，默认下一步就行(默认路径就好)

点击图传设置中的阿里云，主要设置前四个框

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-16.png)

- 设定keyld：创建Buket步骤时，保存的AccessKey ID

- 设定KeySecret：创建Buket步骤时，保存的AccessKey Secret

- 设定存储空间名：就是Buket的创建时候的名称

- 确认存储区域：按照下面进行配置，


查询对应的存储区域

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-17.png)

 保存

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-18.png)

### 测试上传

点击上传区，进行图片的上传测试【随便拖个照片进来测试】

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-19.png)

到达此步骤，那么picgo也下载配置完成了，一定要实现能上传成功，再继续进行下一步的操作 。

## typora设置

打开文件，点击偏好设置

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-20.png)

点击图像，右侧选择上传图片，下面前两个打勾，上传服务选择picgo，再将picgo安装的路径选择上去，最后验证图片上传选项。

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/OSS-21.png)

现在去测试而每次上传的图片都会在【你也可以给自己创建文件目录用来专门存储图片】

## 总结

以上步骤就是使用图床将 typora 数据进行上传到网络上，将图片上传到图床上，这样不管文章发给谁，只要他有网络，他就可以进行访问，使用 阿里云oss 的目的也是为了防止图片进行丢失，效率极高，使用到图床，那就需要用上传图传的工具 picgo 了，使用起来也是非常方便，但是一定要注意配置，一步一步来，不然熬夜肝，就等于浪费时间。 尤其注意点：

- 出于安全考虑使用子用户

- 存储位置就是自己所选择的地域

- 购买对象存储oss花费9元/一年


