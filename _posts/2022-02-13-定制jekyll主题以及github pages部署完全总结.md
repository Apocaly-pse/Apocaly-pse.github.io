# 写在前面

去年就已经开始折腾了jekyll+GitHub Pages的个人站点搭建, 一直以来的想法是自己从零开始进行前端网页的配置, 出发点是好的, 但是我发现最近已经没有时间让我折腾这些了.

恰好前阶段看到网上有人使用jekyll的主题进行博客的配置, 正好省去了自己写css的复杂手续. 下面来看看如何一步步使用主题进行配置, 以及一些实现的小细节, 希望能帮到准备采用jekyll/GitHub pages进行博客搭建的你~~



>   从昨天到今天, 可以说努力是没白费的, 虽然依旧走了很多弯路, 但是已经逐渐熟悉了jekyll主页的配置方法. 真是应了那句老话, "纸上谈来终觉浅, 绝知此事要躬行"..



# 主题的搭建与配置

这里我选择了jekyll-TeXt-theme这个主题, 主要是从这篇博客[^1]中获得了灵感, (这是一款低代码的主题), 话不多数, 当然是先找这个主题的官方文档[^2] (这个主题项目还是一位国内开发者贡献的, 在github上有2k+的star呢)



接下来就是一步一步顺着文档的思路往下走, 这里我想体验一下采用`ruby`的`gem`进行编译和发布的方法, 感觉实现起来不算难, 但是这却成了我折腾到晚上三点的一大绊脚石...

下面主要说通过`gem`以及`bundle`的方式进行主题配置与发布的方法. 





# 给网页添加图标

终于搭建好了网站并发布在了github上, 但是这时候却出现了一个小小的问题, 网页标签页左边的小图标不见了, 取而代之的是一个默认的图标.. 这让有强迫症的我十分不爽, 于是接下来我们继续研究如何添加图标. 

同样地, 我们先进入官方文档[^3]找找, 在这里面还真有很多有价值的信息~具体的配置过程就放在下面啦. 





# 一些额外的主题配置







# 从github迁移镜像到gitee

这里主要参考了[^4], 里面的方法真的是非常详细的, 有了github的actions就不用每次都手动点击同步啦~

主要方法如下: 

1.   提取本地的公钥, 通过终端输入

     ```bash
     cat ~/.ssh/id_rsa.pub
     ```

     查看公钥, 将其复制到

# 主要参考

[^1]:[我的Jekyll博客 | 开放笔记 (goooooouwa.fun)](https://goooooouwa.fun/productivity/2021/03/29/blog-setup.html#博客配置一览);
[^2]:[快速开始 - TeXt Theme (tianqi.name)](https://tianqi.name/jekyll-TeXt-theme/docs/zh/quick-start);
[^3]:[Logo 和 Favicon - TeXt Theme (tianqi.name)](https://tianqi.name/jekyll-TeXt-theme/docs/zh/logo-and-favicon);
[^4]: [github博客自动同步到gitee（保姆级教程）_李梨同学的博客-CSDN博客_github同步到gitee](https://blog.csdn.net/outman_1921/article/details/115454572?spm=1001.2014.3001.5506);

