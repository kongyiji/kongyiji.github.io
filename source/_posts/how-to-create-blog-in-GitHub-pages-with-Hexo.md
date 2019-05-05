---
title: 使用Hexo在GitHub上创建个人博客
date: 2018-08-13 10:58:51
categories: 技术分享
tags:
    - hexo
    - blog
---

# 前言

心血来潮想试试怎么搭个个人博客，三分钟热度。
第一篇文章就先大致写写如何搭建起来这个博客吧。

<!-- more -->

# 基础环境

- OS: Windows 10
- Git version: 2.17.0.windows.1
    - [官网](https://git-scm.com/)
- Node.js version: v8.11.3
    - [官网](https://nodejs.org)

# 安装Hexo

[Hexo官网](https://hexo.io/index.html)上是最新的安装步骤，和本文不符的地方，以官网教程为主。

## npm命令安装Hexo

``` bash
$ npm install hexo-cli -g
$ hexo version
hexo-cli: 1.1.0
```

## 初始化目录

现在新建博客的存放目录，可以先建好目录，也可以初始化的同时新建。

``` bash
$ cd /d D:\
$ hexo init blog # 如果目录已经事先建好，进入目录中，执行hexo init
$ cd blog
$ npm install # 安装依赖包，天朝慢可以使用淘宝镜像，加上参数--registry=https://registry.npm.taobao.org
```

## 试运行

``` bash
$ hexo server
```

浏览器打开 http://localhost:4000/ ，看到**Hello World**的文章，表示安装成功，就这么简单。

# 使用GitHub Pages展示博客

本地的博客能浏览了，就得想办法放到网上了，其实有很多方案，GitHub Pages是其中一个方案。

## 注册GitHub

如果没有GitHub账号，请先注册一个。

## 创建GitHub仓库

登录GitHub后，个人首页相当醒目的地方有个"Start a project"的按钮，点击创建仓库。需要注意的是，**仓库名称必须以`your-username.github.io`的格式命名**，如我的GitHub用户名是kongyiji，那么仓库名称必须是`kongyiji.github.io`。

## 修改`.gitignore`文件

blog文件夹下的`.gitignore`文件中配置忽略上传至GitHub的文件或文件夹，新添加一行

```
package-lock.json
```

## 修改站点配置文件

### 安装[hexo-deploy-git](https://github.com/hexojs/hexo-deployer-git)

``` bash
$ npm install hexo-deployer-git --save
```

### 修改`_config.yml`站点配置文件

```
deploy:
  type: git
  repo: <repository url>
  branch: [branch]
  message: [message]
```

| 参数 | 描述 |
| ----- | ----- |
| repo | 库（Repository）地址 |
| branch | 分支名称。如果您使用的是 GitHub 或 GitCafe 的话，程序会尝试自动检测。 |
| message | 自定义提交信息 (默认为 Site updated: &#123;&#123; now('YYYY-MM-DD HH:mm:ss') &#125;&#125;) |

repo可以使用https或者ssh的方式

## 推送至远程GitHub仓库

``` bash
$ hexo clean
$ hexo generate # 可以缩写成hexo g
$ hexo deploy # 可以缩写成hexo d
```

- 配置的是https的方式，此处会跳出登录框输入GitHub的用户名和密码即可。
- 配置的是ssh方式，需要先配置GitHub上的ssh公钥

至此，博客可以正常显示了。浏览器输入自己的博客地址看看吧。

# 更换主题

hexo有许多丰富多彩的主题，大部分主题都可以在官网的[主题页面](https://hexo.io/themes/)找到。
我用的是next主题，简单耐用。现在GitHub上最新的是6的版本，地址在这：https://github.com/theme-next/hexo-theme-next 。

## 下载主题

blog目录下有个themes文件夹，主题都在这里面，可以存放多个主题，但是只能使用其中一个。

``` bash
$ cd /d D:\blog\
# 可以直接下载，也可以当成子模块来管理
# 使用clone，如果后期环境变更，主题也需要重新拉取
$ git clone https://github.com/theme-next/hexo-theme-next themes/next
# 使用子模块，迁移到新环境后使用 git clone --recurse-submodules 命令可以全部拉取
$ git submodule add https://github.com/theme-next/hexo-theme-next themes/next
```

更新依赖包

``` bash
$ cd themes\next\
$ npm install
```

## 使用主题

使用主题需要将blog文件夹下的站点配置文件`_config.yml`进行修改。

```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next # 将landscape改成next
```

之后重新生成博客，主题就更换了，还有更多的个性化参数可以参考作者的[操作手册](https://theme-next.iissnan.com/)。

``` bash
$ hexo clean
$ hexo g
$ hexo s
```

重新推送到GitHub上后，博客就更新了。

```
$ hexo d
```

# 结语

博客的雏形基本就有了，之后就是个性化的东西了，这些就各自发挥啦。