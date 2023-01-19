---
tags: Frontend CSS
---

# 写在前面

改一下博客的字体显示, 默认的不好看, 这里改成`JetBrainsMono Nerd Font Mono`, 连字字体. 

下载:

[JetBrainsMono Nerd Font Mono](https://github.com/ryanoasis/nerd-fonts/blob/master/patched-fonts/JetBrainsMono/Ligatures/Regular/complete/JetBrains%20Mono%20Regular%20Nerd%20Font%20Complete%20Mono.ttf);

这里我的主题的TeXt, 官方主页:[kitian616/jekyll-TeXt-theme: 💎 🐳 A super customizable Jekyll theme for personal site, team site, blog, project, documentation, etc. (github.com)](https://github.com/kitian616/jekyll-TeXt-theme);

# 更改方法

在本地项目的目录下, 也就是你的`xxx.github.io`这个仓库下, 新建目录:

```bash
mkdir -p _sass/{common,skins}
```

文件:

```bash
vi _sass/common/_variables.scss
```

内容:(其实就是项目中自带的, 由于我的博客使用了远程主题, 所以更改只需要在相应的文件上完成即可)

>   就修改了`font-family-code`. 

```scss
$base: (
  font-family:            (-apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif),
  // 改变正文和代码的字体(连字字体)
  font-family-code:       ('JetBrainsMono Nerd Font Mono', Menlo, Monaco, Consolas, Andale Mono, lucida console, Courier New, monospace),

  font-size-root:         16px,
  font-size-root-sm:      14px,

  font-size-xl:           1.5rem,
  font-size-lg:           1.25rem,
  // 改变正文字体大小
  font-size:              1.1rem,
  font-size-sm:           .85rem,
  // 改变代码字体大小
  font-size-xs:           .85rem,

  font-size-h1-xl:        3.5rem,
  font-size-h2-xl:        2.5rem,
  font-size-h3-xl:        2rem,
  font-size-h4-xl:        1.75rem,
  font-size-h5-xl:        1.5rem,
  font-size-h6-xl:        1.5rem,

  font-size-h1-lg:        3rem,
  font-size-h2-lg:        2rem,
  font-size-h3-lg:        1.75rem,
  font-size-h4-lg:        1.5rem,
  font-size-h5-lg:        1.25rem,
  font-size-h6-lg:        1.25rem,

  font-size-h1:           2.5rem,
  font-size-h2:           1.9rem,
  font-size-h3:           1.5rem,
  font-size-h4:           1.2rem,
  font-size-h5:           1rem,
  font-size-h6:           1rem,

  font-size-h1-sm:        2rem,
  font-size-h2-sm:        1.5rem,
  font-size-h3-sm:        1.35rem,
  font-size-h4-sm:        1.15rem,
  font-size-h5-sm:        1rem,
  font-size-h6-sm:        1rem,

  font-size-h1-xs:        1.05rem,
  font-size-h2-xs:        1rem,
  font-size-h3-xs:        .95rem,
  font-size-h4-xs:        .9rem,
  font-size-h5-xs:        .85rem,
  font-size-h6-xs:        .85rem,

  font-weight:            400,
  font-weight-bold:       700,

  line-height-xl:         2,
  line-height-lg:         1.8,
  line-height:            1.6,
  line-height-sm:         1.4,
  line-height-xs:         1.2,

  spacer:                 1rem,

  border-radius-lg:       .8rem,
  border-radius:          .4rem,
  border-radius-sm:       .2rem
);

$spacers: (
  0:                      0,
  1:                      map-get($base, spacer) * .25,
  2:                      map-get($base, spacer) * .5,
  3:                      map-get($base, spacer),
  4:                      map-get($base, spacer) * 1.5,
  5:                      map-get($base, spacer) * 3
);

$z-indexes: (
  actions: 996,
  mask: 997,
  sidebar: 998,
  modal: 999
);

$layout: (
  header-height:          5rem,
  header-height-sm:       3rem,
  content-max-width:      950px,
  sidebar-width:          250px,
  sidebar-header-height:  3rem,
  aside-width:            220px
);

//     sm       md       lg
// | ------ | ------ | ------ |
// 0       500      1024      -

$responsive: (
  sm:                      0,
  md:                      500px,
  lg:                      1024px
);

$animation: (
  duration:               .4s,
  duration-sm:            .2s,
  timing-function:        ease-in-out
);

$clickable: (
  transition:             all .2s ease-in-out
);

$button-height-xl:        2.8rem;
$button-height-lg:        2.3rem;
$button-height:           1.9rem;
$button-height-sm:        1.5rem;
$button-height-xs:        1.2rem;

$button: (
  padding-y-xl:           ($button-height-xl - map-get($base, font-size-xl)) / 2,
  padding-x-xl:           $button-height-xl / 3,
  padding-y-lg:           ($button-height-lg - map-get($base, font-size-lg)) / 2,
  padding-x-lg:           $button-height-lg / 3,
  padding-y:              ($button-height - map-get($base, font-size)) / 2,
  padding-x:              $button-height / 3,
  padding-y-sm:           ($button-height-sm - map-get($base, font-size-sm)) / 2,
  padding-x-sm:           $button-height-sm / 3,
  padding-y-xs:           ($button-height-xs - map-get($base, font-size-xs)) / 2,
  padding-x-xs:           $button-height-xs / 3,

  pill-radius:            6rem,

  circle-diameter-xl:     $button-height-xl,
  circle-diameter-lg:     $button-height-lg,
  circle-diameter:        $button-height,
  circle-diameter-sm:     $button-height-sm,
  circle-diameter-xs:     $button-height-xs,

  font-weight:            map-get($base, font-weight-bold)
);

$image: (
  width-xl:  20em,
  width-lg:  16rem,
  width:     12rem,
  width-sm:  8rem,
  width-xs:  4rem
);

$menu: (
  horizontal-spacer: 1,
  horizontal-item-vertical-spacer: 1
);
```



然后是:

```bash
vi _sass/skins/_dark.scss
```

这个也是, 只更改一行:(这里是针对dark这个代码高亮主题生效的)

```scss
///
// Skin: Dark
// Author: Tian Qi
// Email: kitian616@outlook.com
///

// code font
$font-family-main: 'JetBrainsMono Nerd Font Mono', Helvetica, Arial, sans-serif;

// main colors
$main-color-1:     #ff9500;
$text-color-1:     rgba(#fff, .8);

$main-color-2:     #ff006a;
$text-color-2:     rgba(#fff, .8);

$main-color-3:     #202020;
$text-color-3:     rgba(#fff, .8);

$main-color-theme-light: rgba(#000, .8);
$main-color-theme-dark:  rgba(#fff, .8);

// page background
$background-color: #121212;

// text colors
$text-color-theme-light-d: #000;
$text-color-theme-light:   #222;
$text-color-theme-light-l: #888;

$text-color-theme-dark-d:  rgba(#fff, .8);
$text-color-theme-dark:    rgba(#fff, .7);
$text-color-theme-dark-l:  rgba(#fff, .5);

$text-color-d:     $text-color-theme-dark-d;
$text-color:       $text-color-theme-dark;
$text-color-l:     $text-color-theme-dark-l;

$text-background-color: rgba(#fff, .05);

// header and footer colors
$header-text-color: $text-color-3;
$header-background: $main-color-3;

$footer-text-color: $text-color-3;
$footer-background: $main-color-3;

// border and shadow colors
$border-color:     mix(#fff, $background-color, 20%);
$border-color-l:   mix(#fff, $background-color, 10%);
$decorate-color:   rgba(#fff, .1);
$mask-color:       rgba(#000, .9);
$select-color:     rgba($main-color-1, .5);

// function colors
$green:           #5baa34;
$blue:            #1c7cd4;
$yellow:          #c9771f;
$red:             #da3d45;
$text-color-function: rgba(#fff, .8);

// logo colors
$mail-color:       #0072c5;
$facebook-color:   #4267b2;
$twitter-color:    #1da1f2;
$weibo-color:      #e6162d;
$google-plus-color:#ea4335;
$telegram-color:   #32afed;
$medium-color:     #000;
$zhihu-color:      #0084ff;
$douban-color:     #42bd56;
$linkedin-color:   #1074af;
$github-color:     #000;
$npm-color:        #fff;

// highlight colors
@import "skins/highlight/tomorrow-night";

```

最后是正文主题的字体

```bash
vi _sass/custom.scss
```

内容为:

```scss
/* start custom scss snippet */
/* 更改正文字体 */
.page__main {
  font-family: 'JetBrainsMono Nerd Font Mono', Times, Menlo, Monaco, Consolas, Andale Mono, lucida console, Courier New, monospace;
}
/* end custom scss snippet */

```



# 本地测试

```bash
bundle exec jekyll serve
```

效果的话, 可以看我的主页了:

[Home - Zorch's Blog (apocaly-pse.github.io)](https://apocaly-pse.github.io/);

