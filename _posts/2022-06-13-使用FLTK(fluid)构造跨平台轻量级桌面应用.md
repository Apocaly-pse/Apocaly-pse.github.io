---
tags: FLTK C++
---

# 写在前面

最近能抽出时间完成一些小项目, 顺便学一下FLTK, 之前也试过gtkmm,qt等等, 但是qt显得过于冗杂了, gtkmm的glade动不动就闪退(可能是macOS的原因), 于是还是捡起尘封已久的FLTK教程学一下, 参考了y2b的一个视频, 虽然很老, 但是还是有很多的值得学习的地方, 特别是通过fluid构建应用这块.



# 安装

>   macOS直接通过brew安装, 采用:
>
>   ```bash
>   brew install fltk
>   ```
>
>   其他平台类似, Windows建议采用msys2, Linux当然有包管理器加持,安装简简单单.

# 界面介绍

<img src="https://s2.loli.net/2022/06/13/eXc2txMSQZGOlT5.jpg"/>

<img src="https://s2.loli.net/2022/06/13/vT2gqFe6EfkZSVJ.jpg"/>

第一个界面名为`widgetbar`, 默认是不显示的, 可以从设置里面打开.