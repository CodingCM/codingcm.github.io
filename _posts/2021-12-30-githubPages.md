---
title: 使用github.io搭建个人博客
category: [toolswiki, git]
sidebar:
    nav: TOOL
---

## 1 新建Github仓库

本Wiki网页模板为[JEKYLL-TEXT-THEME](https://github.com/kitian616/jekyll-TeXt-theme)。根据该项目提供的[安装教程](https://tianqi.name/jekyll-TeXt-theme/docs/zh/quick-start)，直接fork该项目，并修改仓库名称为自己的github（以username为例）用户名即可通过访问username.github.io来访问自己的Github Pages。

## 2 关联本地仓库

通过 `git clone https://github.com/username/username.github.io.git`将仓库下载到本地，即可在本地自定义网页或进行wiki记录。在完成相关编辑后，可以使用jekyll先在本地进行调试再push至github仓库。具体方法可以参考模板[安装教程](https://tianqi.name/jekyll-TeXt-theme/docs/zh/quick-start)中的**本地预览**章节以及Github Pages官方提供的[Jekyll安装教程](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)。

注：第一次进行本地仓库与GitHub远程仓库关联可以参考[此Wiki](https://codingcm.github.io/toolswiki/git/2021/12/30/githubKey.html)进行配置。

## 3 Jekyll模板相关

使用Jekyll编译的模板有固定的规则，这里做一些经验性的总结。

（1）_posts是存放wiki或blog内容（即Markdown文件）的文件夹，所有的.md文件都放在这个目录下；

（2）_layouts文件夹下存放html格式的布局文件；

（3）_site是在使用jekyll编译过生成的文件夹，其中生成的文件才是网页真正调用的文件。似乎对网页的布局做出改变后，都需要将__site文件夹删除掉并重新编译才会生效；

## 4 文章内容分类

此部分内容参考了[此文章](https://smartadpole.github.io/tool/blog/2021/01/18/TeXt-theme-head.html)。

在该模板下，可以通过导航栏来分类不同的文章。参考模板安装教程，可以通过修改`_data/navigation.yml`配置文件来增加导航项目。以本站的‘工具wiki’栏为例，在header下新增titles内容如下：

```yml
  - titles:
      # @start locale config
      en      : &EN       Tools Wiki
      zh-Hans : &ZH_HANS  工具Wiki
      zh      : *ZH_HANS
      # @end locale config
    url: /toolswiki/index.html
```

此外，在根目录下新建toolswiki文件夹以及相应的index.html文件，内容参考[源文件](https://github.com/CodingCM/codingcm.github.io/blob/master/toolswiki/index.html)，其关键在于`if cat[0] == "toolswiki"`将含有toolswiki标签的文章进行筛选。那么，后续所有在_post中新增的文章，只要含有toolswiki标签，就是被显示在该导航栏下。在.md文件的头部加上category，如：

```
---
title: 使用github.io搭建个人博客
category: [toolswiki, git]
sidebar:
    nav: TOOL
---
```

这里，此文章包含toolswiki和git两个标签。那么该文章标题就会被显示在‘工具Wiki’导航栏下。

