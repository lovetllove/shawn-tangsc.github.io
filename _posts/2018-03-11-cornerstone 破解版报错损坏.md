---
layout: post
title: "Mac SVN管理软件Conerstone破解版安装后提示“已损坏”问题"
date: 2018-03-11
description: "Mac SVN管理软件Conerstone破解版安装后提示“已损坏”问题"
tag: 工具
---

#Mac SVN管理软件Conerstone破解版安装后提示“已损坏”问题

> 参考：http://blog.csdn.net/wsyx768/article/details/52756782


##导致问题原因
软件有经过了汉化或者破解，所以可能被Mac认为「已损坏」

##解决问题办法

 系统偏好设置 -> 安全性与隐私 -> 通用 -> 选择“任何来源”
 
 * 如果通用里面没有‘任何来源’这个选项，需要在终端输入：

```
sudo spctl --master-disable
```

* 如果需要隐藏‘任何来源’选项,在终端输入。

```
sudo spctl --master-enable
```

## 结语

* 这样，你应该就可以通过网上下载的破解版，来使用svn了.
* mac 其实是自带终端的，也可以直接在终端里操作svn。
