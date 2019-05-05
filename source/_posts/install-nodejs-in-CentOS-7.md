---
title: install nodejs in CentOS 7
date: 2019-05-05 17:42:05
categories: 技术分享
tags:
    - nodejs
    - CentOS
---

# 基础环境

- Node.js版本：8.11.1
- 系统版本：CentOS 7.4.1708 X64

<!-- more -->

# 安装Node.js

使用EPEL仓库安装
```shell
# 安装EPEL仓库
yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
# 安装Node.js和npm
sudo yum install nodejs npm --enablerepo=epel
```

使用yum仓库文件安装
```shell
# 安装yum源文件
curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
# 安装编译模块
sudo yum install gcc-c++ make
# 安装Node.js和npm
sudo yum install nodejs
```