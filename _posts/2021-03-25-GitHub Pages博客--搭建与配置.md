---
layout: post
author: zorchp
---


@[toc]

#  写在前面

最近想尝试一下GitHub提供的GitHub Pages功能,通过GitHub强大的服务器支持,可以免费搭建自己的个人站点(个人博客),看了很多博客和教程,特别是Jekyll的官方文档,终于摸索出来一套属于Mac的个人站点搭建方法.

我主要使用了Jekyll作为渲染静态页面的引擎,本来是想直接拷贝别人做好的,然后自己再进行修改,但是感觉这样还是学不到东西,所以还是自己亲自动手实现一下啦~

> 本文部分参考了阮一峰老师的博客[^2]以及Jekyll的step-by-step-tutorial文档[^3],(不得不说Dash是真的不错,校园网时好时坏的情况下还是要靠离线文档挑大梁了).教程部分主要选取了jekyll的官方教程[^3],教程是英文的,不过写的很详细,想深入学习的话还得看官方文档.



# 前期准备

## GitHub

首先你需要有一个GitHub账号,并且创建一个名为`<username>.github.io`的仓库,这样可以通过直接在浏览器中访问`<username>.github.io`来进入自己的站点;

创建这个仓库之后,就可以clone到本地,然后进行自己的更改了;

接下来是Jekyll的配置.

## Jekyll

一开始我以为直接使用Mac自带的ruby环境就可以下载Jekyll和bundler了,但是一番操作之后发现并不行...无奈对ruby不是很了解,走了弯路,之后我发现在Jekyll的官方安装文档[^1]中有关于macOS如何安装Jekyll的介绍,很快就完成了Jekyll及其依赖的安装.

首先你需要安装命令行开发工具(终端输入`xcode-select --install`),完成之后下载本地编译版brew,这个我在之前已经写过一篇文章,大家可以参考一下[《m1 MBA配置Homebrew环境+国内源配置》](https://blog.csdn.net/qq_41437512/article/details/112435816),然后输入:

```bash
brew install ruby
# 添加ruby路径到环境变量
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

然后在终端查看ruby安装位置与版本信息:

```bash
which ruby 
ruby -v
```

使用ruby的包管理器`gem`安装Jekyll及bundle(这一步如果速度慢的话可以添加ruby的国内源镜像)

```bash
# 移除默认镜像
sudo gem sources --remove https://rubygems.org/
# 添加国内镜像
sudo gem sources -a http://gems.ruby-china.com/
# 查看镜像列表
gem sources -l
# 安装bundler和jekyll
sudo gem install --user-install bundler jekyll
# 安装缺失的包(也可以视情况安装,看看后面会不会因为这个包而报错)
sudo gem install webrick
# 将gem安装的包路径添加到环境变量
echo 'export PATH="$HOME/.gem/ruby/3.0.0/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

最后可以通过`gem env`检查路径是否正确(正确的话会在路径中显示安装gem包的家目录)

# 搭建个人站点(博客)

完成上述的几个步骤后,就可以创建自己的个人站点了,这里需要先创建几个文件(文件夹)如下:

1.  `_config.yml`: 配置文件(yaml)
2.  `_layouts/default.html`: 网页模板文件(html)
3.  `_posts/2021-03-23-blog1.md`: 你的博客文章就放在这里(可以用HTML,也可以用Markdown)
4.  `index.md`: Blog的Markdown主页

这几个文件夹中的内容已经足够渲染出一个包含基本内容的博客了,但是如果想实现更为复杂的功能就还需要新建一些配置文件/文件夹,后面我们会说到.

# 基本的四个文件

## 1. _config.yml配置文件

该文件用来**配置网页的标题等信息**,第一行设置根目录位置,第二行选择Markdown为博客编辑语言,之后是标题和描述信息.

```yaml
baseurl: /
markdown: kramdown

title: My Homepage
description: share some useful skills and tips
```

## 2. _layouts/default.html网页模板文件

单下划线+layouts作为文件目录,里面的html文件都是你的博客的模板,`default`是一个所有页面都要遵循的主模板,这个模板主体使用的是HTML标记语言,双大括号或者大括号内包含百分号这样的表达式是一种名叫Liquid的模板语言.

除了default模板,你还可以创建`post.html`文件作为`_posts`文件夹内博客文章的模板,只需要在文档的开头使用三个短线包裹的部分中加入yaml配置即可,如下是基本的`default.html`文件:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>{{ page.title }}</title>
</head>
<body>
  {{ content }}
</body>
</html>
```

下面是博客内容的模板文件`post.html`,这里先在文件头部指定了布局为`default`,相当于一种对default的继承,下面实现了显示文章的标题,日期以及作者的功能,最后面的`{{content}}`是内容(正文)部分.

```html
---
layout: default
---

<h1>{{ page.title }}</h1>
<p>
  {{ page.date | date_to_string }} - {{page.author}}
</p>

{{ content }}
```

## 3. _posts/2021/03/23-blog1.md博客文章

这个就是博客的主文件了,只需要声明布局及作者(可选)就可以:

```markdown
---
layout: post
author: xxx
---

## 写在前面

这是我的第一个个人博客
```



## 4. index.md网页主页

这里是一个你网站的主页,也就是`xxx.github.io`这个网站打开时显示的默认页面,这里我采用了Markdown语言,大家也可以采用可定制性更高的html.`{{site.title}}`会输出当前的页面标题.

```markdown
---
layout: default
title: 欢迎来到我的博客

---

{{site.title}}

```



# 用来扩展的其他文件

除了上面的四个基本文件,想要实现更为复杂的页面样式的话,还需要添加一些其他的文件,如图片\CSS样式等,这就需要一些其他的文件来配置了.

## CSS样式

首先来看看CSS样式如何在你的网站中实现,一般来说网站的建设要遵循样式与内容分离的,所以最好将你的CSS样式独立出来,便于之后的维护.

需要新建两个文件:

### `_sass/main.scss`文件

保存css样式,例如可以添加如下内容:

```css
.current {
  color: green;
}
```



### `assets/css/styles.scss`文件

用于指出css文件存放的位置,其内容为

```css
---

---

@import "main";
```

这里的yaml头是必要的,用来声明jekyll引擎要处理的文档,下面的语句代表导入上面的`main.scss`样式文件.

然后我们还需要将CSS的路径添加到default.html模板文件中,这样才可以正确导入CSS,即在default.html文件中的`<head></head>`标签中加入:

```html
  <!-- 添加CSS路径到当前文件 -->
  <link rel="stylesheet" href="/assets/css/styles.css">
```



## 设置博客列表及关于页面

有了主页还不够,还需要加上能够显示所有博客的一个列表,在站点根目录下新建一个`blog.html`文件,输入以下内容:

```html
---
layout: default
title: Blog
---

<h1>最新文章</h1>

<!-- 链接到全部博客(for循环遍历) -->
<ul>
    {% for post in site.posts %}
    <li> 
        <h3>{{ post.date | date_to_string }}
        <a href="{{ post.url }}">{{ post.title }}</a></h3>
        <!-- 默认内容的第一段 -->
        {{ post.excerpt }}
    </li>
    {% endfor %}
</ul>
```

这里使用Liquid的for循环遍历_posts目录下的所有文章并输出标题及文章的默认第一段(相当于预览).

之后是一个about页面,同样在根目录下新建文件`about.md`,这里可以放上你的个人介绍等内容,内容如下:

```markdown
---
layout: default
title: About
---

# About page

This page tells you a little bit about me.
```



## 导航栏

一般的HTML都需要一个导航栏来实现页面之间的跳转,HTML就实现了这一基本功能,这时候需要在网站的根目录下新建`/_includes/navigation.html`文件,文件的内容为:

```html
<!-- 设置导航栏,并使当前网页高亮显示 -->
<nav>
  {% for item in site.data.navigation %}
    <a href="{{ item.link }}" {% if page.url == item.link %}{% endif %}>
      {{ item.name }}
    </a>
  {% endfor %}
</nav>
```

这里使用Liquid设置了一个for循环,用来遍历主页/博客页面及关于页面.

要实现正常的链接跳转,还需要新建一个配置文件`_data/navigation.yml`,即yml文件,用来告诉导航栏要显示的内容以及超链接:

```yaml
- name: Home
  link: /
- name: Blog
  link: /blog.html
- name: About
  link: /about.html
```

同时,在这里就可以应用我们之前提到的CSS样式文件了,在navigation.html文件的第四行加入`class="current"`,即我们之前设置的current节点的样式,如下:

```html
    <a href="{{ item.link }}" {% if page.url == item.link %}class="current"{% endif %}>
```





## 点缀:添加主页及标签页的图标

一般在浏览器中,打开的标签页左边都会有个小图标,我们的网站也可以实现这个功能,只需要将制作好的ico图标文件放在`assets/images/favicon.ico`即可,jekyll就会找到图标的位置,然后在default.html的`<head></head>`中加入:

```html
  <!-- 下面两行用于生成主页标签页以及子网页标签页上面的小图标 -->
  <link rel="shortcut icon" href="/assets/images/favicon.ico" type="image/x-icon"/>
  <link rel="bookmark" href="/assets/images/favicon.ico"/>
```

就可以正确显示标签页图标了.



最后的default.html文件如下:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <!-- 添加CSS路径到当前文件 -->
  <link rel="stylesheet" href="/assets/css/styles.css">
  <title>{{ page.title }}</title>

  <!-- 下面两行用于生成主页标签页以及子网页标签页上面的小图标 -->
  <link rel="shortcut icon" href="/assets/images/favicon.ico" type="image/x-icon"/>
  <link rel="bookmark" href="/assets/images/favicon.ico"/>
</head>
<body>
  <!-- 设置导航栏 -->
  {% include navigation.html %}

  {{ content }}
</body>
</html>
```



# 文件树目录

这里是最后的文件树目录,还是挺复杂的..

注意这里是没有启动过`Jekyll server`服务的,开启之后还会生成一些文件夹,如`_site`文件夹(jekyll引擎生成的站点文件都在这里)和`.jekyll_cache`文件夹(缓存文件).

```
.
├── README.md
├── _config.yml
├── _data
│   └── navigation.yml
├── _includes
│   └── navigation.html
├── _layouts
│   ├── default.html
│   └── post.html
├── _posts
│   ├── 2021-03-25-blog1.md
│   └── 2021-03-26-blog2.md
├── _sass
│   └── main.scss
├── about.md
├── assets
│   ├── css
│   │   └── styles.scss
│   └── images
│       └── favicon.ico
├── blog.html
└── index.md
```



# Jekyll本地服务部署

有两种生成静态网页的方式:

`cd`到当前目录(即`<username>.github.io`),可以采用`jekyll build`构建静态网页或者`jekyll server`启动服务,这里还是建议直接用`server`,开启服务,然后就可以在浏览器输入`http://localhost:4000`进入你的静态网页了.

使用control+C即可关闭服务,在Jekyll的官方文档中提到: **如果修改了config.yml配置或其他yml文件的配置**,那就需要control+C中止服务并重启服务来刷新配置.

# 最后成果与小结

---图片图片图片图片

还是单调了点,之后慢慢学前端的知识再丰富自己的网站吧.

最后附上我的GitHub Pages Blog:[apocaly-pse.github.io](https://apocaly-pse.github.io).

# 参考

[^1]: [Jekyll on macOS](https://jekyllrb.com/docs/installation/macos/);
[^2]: [搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html);
[^3]: [Jekyll Step by Step Tutorial](https://jekyllrb.com/docs/step-by-step/01-setup/);