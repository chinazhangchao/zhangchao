---
title: "Sublime Text 3增加最近打开的文件数量（Windows）"
date: 2019-09-21T09:43:21+08:00
type: posts
categories: [工具]
keywords: ["sublime text"]
tags: ["sublime text"]
---

Sublime Text 3 默认只存储7个最近打开的文件、7个最近打开的文件夹。

有时觉得不够用，想增大这个数值，该怎么做呢？本文以Windows系统为例说明如何修改。（Mac请参考[Sublime Text 3增加最近打开的文件数量（Mac）]({{< ref "sublime-text-3-increase-recent-file-number-mac.md" >}})）
<!--more-->
1. 找到Sublime Text 3安装目录下的`Default.sublime-package`文件，在我电脑上的路径为：`D:\Program Files\Sublime Text 3\Packages\Default.sublime-package`，复制一份该文件，把文件扩展名改为zip，比如改为：`Default.zip`。
<img src="/tool/sublime-text-3-increase-recent-file-number-windows/1.png" style="width:600px;"/>

1. 解压该zip文件，在解压出的目录中找到文件`Main.sublime-menu`，用文本编辑器打开：
<img src="/tool/sublime-text-3-increase-recent-file-number-windows/2.png" style="width: 600px;"/>

1. 找到如图所示的文本，增加几行`{ "command": "open_recent_file", "args": {"index": n } },`。
<img src="/tool/sublime-text-3-increase-recent-file-number-windows/3-1.png" style="width: 600px;"/>
例如把最近打开的文件和文件夹数量都改为15：
<img src="/tool/sublime-text-3-increase-recent-file-number-windows/3-2.png" style="width: 600px;"/>

1. 保存修改的文件`Main.sublime-menu`，找到Sublime Text 3的配置文件目录，在我电脑为`C:\Users\zhangchao\AppData\Roaming\Sublime Text 3\Packages\`，新建文件夹`Default`，把修改后的`Main.sublime-menu`文件复制到`Default`目录中即可。
<img src="/tool/sublime-text-3-increase-recent-file-number-windows/4.png" style="width: 600px;"/>
