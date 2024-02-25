---
layout: post
title: "Jekyll Learning #04 - Mac 系统上安装 Jekyll"
subtitle: '不能直接使用Mac自带的ruby环境就可以下载Jekyll和bundler。'
date: 2024-02-25 19:00:00
author: "WePro"
copyright-statement: 
hidden: false
header-img: "img/post/post-bg-mac-jekyll.jpg"
excerpt: Mac 系统上安装 Jekyll
catalog: true
tags:
  - Jekyll
  - Mac
  - M1
---

一开始我以为直接使用Mac自带的ruby环境就可以下载Jekyll和bundler了,但是一番操作之后发现并不行。无奈对ruby不是很了解,走了弯路,之后我发现在Jekyll的官方安装文档<sup>[1](https://jekyllrb.com/docs/installation/macos/)</sup>中有关于macOS如何安装Jekyll的介绍,很快就完成了Jekyll及其依赖的安装。

## 安装 Brew 环境

首先你需要在终端输入，安装命令行开发工具：

```bash
xcode-select --install
```

完成之后下载本地编译版brew,大家可以参考一下[《Mac配置Homebrew环境+国内源配置》](https://www.wepro.uk/2024/02/24/Mac%E9%85%8D%E7%BD%AEHomebrew%E7%8E%AF%E5%A2%83+%E5%9B%BD%E5%86%85%E6%BA%90%E9%85%8D%E7%BD%AE/)。

## 安装Ruby

使用brew安装:

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

## 安装gem

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

最后可以通过该代码，检查路径是否正确(正确的话会在路径中显示安装gem包的根目录)

```bash
gem env
```

查看`gem`版本号

```bash
gem –version
```

对比下[官网](https://rubygems.org/pages/download)的版本。可以使用以下命令更新

```bash
sudo gem install --system
```

## 安装jekyll

```bash
sudo gem install jekyll
```

```bash
sudo gem install bundler
```

## 安装依赖

```bash
sudo gem install jekyll-paginate
sudo gem install jekyll-gist
```

## 查看jekyll

检查jekyll是否成功安装

```bash
jekyll -v
```

## 开启服务

找到安放服务的文件夹，右键进入终端模式，输入

```bash
jekyll serve
```

## 参考

[Jekyll on macOS](https://jekyllrb.com/docs/installation/macos/)

[Step by Step Tutorial](https://jekyllrb.com/docs/step-by-step/01-setup/)

[m1芯片Mac安装jekyll+搭建GitHub pages个人博客站点](https://blog.csdn.net/qq_41437512/article/details/115179412)