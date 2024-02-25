---
layout: post
title: "Jekyll Learning #03 - Windows 系统上安装 Jekyll"
subtitle: '本地Windows上搭建一个服务，方便调试由 Jekyll 生成的网站效果。'
date: 2024-02-24 22:00:00
author: "WePro"
copyright-statement: 
hidden: false
header-img: "img/post/post-bg-win-jekyll.jpg"
excerpt: Windows 系统上安装 Jekyll
catalog: true
tags:
  - Jekyll
  - Windows
---

搭建完个人的博客网站之后，想修改网站内容，使用代码修改提交的方式有点麻烦。所以可以本地Windows上搭建一个服务，方便调试由 Jekyll 生成的网站效果。

Jekyll 是由 Ruby 语言开发而成的网站搭建工具，其可完成由 Markdown 代码仓库生成网站的流程。

## 安装 Ruby 环境

[官网](https://rubyinstaller.org/downloads/) 下载RubyInstaller(WITH DEVKIT)。
>注意：所有的勾都要选上（默认）。

无难点安装后 验证命令：

```dos
ruby -v
```

## 安装RubyGems
 

Windows中下载[ZIP](https://rubygems.org/pages/download)，
下载后解压到任意路径。打开Windows的cmd界面，
输入命令： 
```dos
$ cd 解压的路径
```

## 切换镜像源

```dos
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/

gem sources -l
```

## 安装Jekyll
 
```dos
gem install jekyll
```

## 安装jekyll-paginate
 
```dos
gem install jekyll-paginate
```

## 验证 jekyll

```dos
jekyll -v
```

## 安装 Bundler
 
```dos
gem install bundler 
```

`安装 一个名为 Bundler 的程序 —— 用于自动安装其他所需的程序`

## 本地启动服务

直接找到加载服务的文件夹，选中后，右键进入cmd界面

```dos
jekyll serve 
```

## 查看网站
 浏览器直接输入`127.0.0.1:4000` 或 `localhost:4000`


>注意：如端口被占用修改端口 

```dos
jekyll serve -P 5555
```

## 参考

[Jekyll搭建个人博客](https://www.jianshu.com/p/245aabdace05)