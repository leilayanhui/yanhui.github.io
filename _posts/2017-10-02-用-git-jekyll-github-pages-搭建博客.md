---
title: 用 Git Jekyll Github Pages 搭建博客
excerpt_separator: <!--more-->
categories:
 - programming
tags:
---

# 三大工具 Git, Jekyll, Github Pages
使用这三个工具搭建博客，既能专心写博客，又能达到高度自己定义，无广告，若你还会 css，JavaScript，还能自己设计博客式样。

**Git**：用于管理文档版本，即使你不断修改、更新自己的博客，也能用 git 找回原来的内容。无需上网就能完成。

**Jekyll**：你可以把它理解成一个文本转换器，使用 markdown 专心写作，然后交由 jekyll 帮你转换成静态的网页，有标题、发布日期、装饰、字体、动画效果等的网页页面。

**Github Pages**：想要世界各地的人们看到你的博客，你需要将其发布到公共网络平台上，类似新浪博客、简书。Github Pages 就是这样的平台，除了博客，还能发布各种开发项目。

<!--more-->

我的系统环境：windows 10 64bit, windows subsystem linux, ubuntu 16.04

# 注册 GitHub Pages
登陆 [Github](https://github.com/)，完成注册。进入自己的页面，点击右上角的加号 New repository。 在 Repository name 填入 username.github.io，勾选 Initialize this repository with a README。进入该仓库，点击 Settings，拉到 GitHub Pages，在 Source 中选择 master branch。

# 安装 Git
ubuntu 安装命令

    $ sudo apt-get install git

记得查官方安装文档哦，各种系统都有说明。

# 安装 Jekyll
## 安装 Ruby
Jekyll 是基于 Ruby 开发的，所以要先安装 Ruby。Ruby 和 Python 一样有很多版本，所以最好先安装 Ruby 的版本管理软件，如 rvm，再通过 rvm 安装 ruby。
```
# 先安装 GPG keys
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

# 然后安装 rvm，ruby
$ \curl -sSL https://get.rvm.io | bash -s stable --ruby

$ echo 'source ~/.rvm/scripts/rvm' >> ~/.bashrc
$ source ~/.bashrc
$ ruby --version
ruby 2.4.1p111
```

ubuntu 的安装过程中提示你 `Run command as login shell`，在 Bash 中使用 `bash -l` 登陆。

## 安装 Jekyll
```
$ gem install bundler
$ cd ~
$ mkdir myblog
$ cd myblog
$ git init
Initialized empty Git repository in /home/.../myblog/.git

$ jekyll new .
Running bundle install in /home/.../myblog/.git
....
New jekyll site installed in /home/.../myblog/.git

$ bundler exec jekyll serve
```
Jekyll 会生成一个内网地址，在浏览器中复制该地址，你就可以在本地预览自己的博客。只是此时的博客非常简陋，想要自己的博客美美的，需要使用主题模板。

## 主题模板
在 http://jekyllthemes.org/ 上挑选模板，我选了 [NexT](https://github.com/Simpleyyt/jekyll-theme-next). 首先将模板克隆到本，再新建文件夹 myblog，将模板内的所有文件复制到 myblog，新建 .gitignore，添加自己的 gihub 博客仓库，修改 `_config.yml` 调整模板配置，推送 github，博客搭建成功~
```
$ git clone https://github.com/Simpleyyt/jekyll-theme-next.git
$ mkdir myblog

# 将 jekyll-theme-next 下的所有文件复制到 myblog
$ cp -r jekyll-theme-next/* myblog
$ cd myblog
$ git init

# 新建 .gitignore
$ touch .gitignore
```
打开编辑器，将 jekyll-theme-next/.gitignore 的内容，复制到 myblog/.gitignore 中
```
# 添加自己的博客仓库
$ git remote add origin https://github.com/......
$ git fetch origin
$ git merge origin/master
```
打开编辑器，根据模板文档修改 `_config.yml`。查看自己的 Gemfile 文件，内容是否如下
```
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
```
发布的博文都放在 `_post` 文件夹，主题模板会自带一些测试页面，把它们删除，添加自己的文章。回到命令行
```
$ cd myblog
$ git add .
$ git commit -m "Init commit"
$ git push origin master
```
打开自己的博客仓库，点击 Setting，拉到 GitHub Pages，点自己的博客地址，博客搭建成功。

## 添加表情
使用 [jemoji](https://github.com/jekyll/jemoji) 插件。在自己的 Gemfile 添加 `gem 'jemoji'`
```
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
gem 'jemoji'
```
编辑器打开 `_config.yml`，搜索 gems，添加 `- jemoji`
```
gems:
  - jemoji
```

你一定会在安装过程碰到很多问题，别慌，使用 google！科学上网，用英文搜索。根据自己的折腾，如果用 google 找不到答案，只有两个原因：一，没有问对问题，换一个关键词；二，你找到了答案，可惜小白看不懂…… 怎么办？既然是小白，多折腾咯


**参考**
- [Git - Installing Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [GitHub Pages](https://pages.github.com/)
- [RVM install](https://rvm.io/rvm/install)
- [Using RVM in bash for Ubuntu on Windows](https://rvm.io/integration/ubuntu-on-windows)
- [Setting up your GitHub Pages site locally with Jekyll]( https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)
- [Jekyll](http://jekyllrb.com/docs/quickstart/)
- [Jekyll 搭建静态博客 — 漠然](https://mritd.me/2016/10/09/jekyll-create-a-static-blog/#33启动并修改)
