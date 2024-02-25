---
layout: post
title: "Jekyll Learning #02 - Mac配置Homebrew环境+国内源配置"
subtitle: 'M系列芯片搭载的Mac在安装Jekyll之前配置Homebrew环境'
date: 2024-02-24 21:00:00
author: "WePro"
copyright-statement: 
hidden: false
header-img: "img/post-bg-google-analysis.jpg"
excerpt: Homebrew Enviroment
catalog: true
tags:
  - Jekyll
  - Homebrew环境
---


## Homebrew本地编译版安装

由于原生版本（beta）的brew已经发布，正好看到国外有位大佬的博客上面有详细的多平台版本brew配置<sup>[1](https://soff.es/blog/homebrew-on-apple-silicon)</sup>，在这里先借鉴一下.后来发现在官方文档中也有此命令<sup>[2](https://docs.brew.sh/Installation#untar-anywhere)</sup>。

首先打开终端，运行下面的命令：

```bash
sudo mkdir -p /opt/homebrew
sudo chown -R $(whoami):staff /opt/homebrew
cd /opt
curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 
```

第四步是brew从git仓库下载源码解压并进行本地编译，网速快的话也不会等太久（我这里用了大概5分钟），注意这里安装的brew就是原生版本了，但是很多软件还只能在x86下运行，所以还需要安装Rosetta2转译版本的brew。

>上面的brew安装在了系统的/opt/homebrew目录中，这也是brew官方建议的安装目录，这样可以和下面的转译版区分开。

## Homebrew Rosetta2转译版安装

这里面的坑就比较多了，国内由于无法访问 `raw.githubusercontent.com`,官网提供的命令

```bash
arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

也不能使用。但是没关系，我们可以通过修改国内源镜像的方式加快访问速度。在这里主要参考了3，虽然版本更新迭代很快，一些命令已经不能用了，但是这篇文章的思路给了我很大启发，下面具体来看。

首先需要将上面命令中的网址在浏览器打开，（可能需要一些🪜插件,ghelper就可以）全选复制保存为 `install.sh` 文本文件，然后编辑器打开，这里需要修改三个地方：

```bash
HOMEBREW_CORE_GIT_REMOTE="https://github.com/Homebrew/homebrew-core"
HOMEBREW_BREW_GIT_REMOTE="https://github.com/Homebrew/brew"
```

将上面的网址换成（中科大镜像网址）：

```bash
HOMEBREW_CORE_GIT_REMOTE="http://mirrors.ustc.edu.cn/homebrew-core.git"
HOMEBREW_BREW_GIT_REMOTE="http://mirrors.ustc.edu.cn/brew.git"
```

>P.S.： 可能新版安装命令会有变化，但是思路不变，就是替换网址。需要注意后面的对应关系，即homebrew-core,linuxbrew-core,brew这三个，可以直接在[镜像站](https://mirrors.ustc.edu.cn/)查看并复制链接。

保存后（我将其保存到桌面了），在终端运行(`arch -x86_64`意思是采用Rosetta2方式运行命令)：

```bash
arch -x86_64 /bin/bash ～/Desktop/install.sh
```

就可以等待安装完成了。

## 设置环境变量

然后`vim ~/.zshrc`，打开zsh的配置文件（Mac big sur终端已经换成zsh shell ），将下面的两行添加到配置文件：

```bash
export PATH="/opt/homebrew/bin:/usr/local/bin:$PATH"
alias ibrew='arch -x86_64 /usr/local/bin/brew'
```

之后进入终端，输入`source ~/.zshrc`，就能在终端中使用brew了，建议还是先在GitHub主页的[issue](https://github.com/Homebrew/brew/issues/7857)中查看一下是否有arm版本的软件，再用`brew`命令进行安装，否则还是`ibrew`吧。

然后需要验证一下：

```bash
brew --repo
ibrew --repo
```
如果显示为

```bash
/opt/homebrew
/usr/local/Homebrew
```

就说明安装位置对了（用`which brew`也可以显示）。

安装包只需终端执行(大多软件包还未适配，所以这里用`ibrew`)：

```bash
ibrew install <package name>
```

就可以了。

## 换源

由于安装包都是从GitHub下载的，访问速度比较慢，下面以中科大的brew源为例提高brew安装包的速度。

直接在终端执行：

```bash
# 修改brew镜像源
git -C "$(brew --repo)" remote set-url origin https://mirrors.ustc.edu.cn/brew.git
# 修改homebrew-core镜像源
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
# 更新
brew update
```

即可。可以使用如下的命令查看源：（brew的话：去掉i即可）

```bash
# 查看brew镜像源
git -C "$(ibrew --repo)" remote -v
# 查看homebrew-core镜像源
git -C "$(ibrew --repo homebrew/core)" remote -v
```

更新源后的结果如下：

```bash
origin	http://mirrors.ustc.edu.cn/brew.git (fetch)
origin	http://mirrors.ustc.edu.cn/brew.git (push)

origin	http://mirrors.ustc.edu.cn/homebrew-core.git (fetch)
origin	http://mirrors.ustc.edu.cn/homebrew-core.git (push)
```

## 结语
1、在使用本地编译版brew安装软件（更换源后），可能会出现SHA256不匹配、软件的安装目录不对等问题，这里还是建议先查询一下GitHub的[issue](https://github.com/Homebrew/brew/issues/7857)，比如brew安装优化版的numpy的时候就会有SHA256的error出现，这时候采用ibrew安装就不会出现类似的问题。

2、一定要多用`which`命令查看当前使用的软件是在哪里，`/opt`的话就是在本地编译版中，`/usr`就是在Rosetta2转译版中，防止使用时有未知错误发生。

3、之后随着开发者的努力，brew肯定会完全适配m1.

4、[Homebrew](https://doesitarm.com/kind/homebrew/)这个网址可以很方便地查询brew包的支持情况。

## 主要参考

[Homebrew on Apple Silicon](https://soff.es/blog/homebrew-on-apple-silicon)

[Homebrew Documentation.](https://docs.brew.sh/Installation#untar-anywhere)

