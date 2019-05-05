---
title: Windows 10上多Git账号
date: 2019-05-05 17:14:45
categories: 技术分享
tags:
    - git
    - win10
---

# 基础环境

- OS版本：Windows 10
- Git版本：2.19.2.windows.1

# 配置多Git账号步骤

一般有多个Git账户的时候需要配置，比如需要同时使用GitHub和GitLab

<!-- more -->

**1. 生成GitHub的SSH公钥和私钥**

```shell
ssh-keygen -t rsa -b 4096 -C "githubmail@github.com"
# 将id_rsa重命名成id_rsa_github
```

**2. 生成GitLab的SSH公钥和私钥**

```shell
ssh-keygen -t rsa -b 4096 -C "gitlabmail@gitlab.com"
# 将id_rsa重命名成id_rsa_github
```

**3. 确认公钥和私钥在正确的文件夹中**

确保`id_rsa_github`、`id_rsa_github.pub`、`id_rsa_gitlab`和`id_rsa_gitlab.pub`四个文件在`C:\Users\your_user_name\.ssh`文件夹当中。

**4. 将公钥分别添加至对应的服务器上**

公钥文件指的是以`.pub`结尾的文件，此处是`id_rsa_github.pub`和`id_rsa_gitlab.pub`。

**5. 创建config文件**

在`C:\Users\your_user_name\.ssh`文件夹中创建config文件

```
# GitHub配置
Host github.com                 
HostName github.com
IdentityFile C:\\Users\\your_user_name\\.ssh\\id_rsa_github
PreferredAuthentications publickey
User github_username

# GitLab配置
Host gitlab.com
HostName gitlab.com
IdentityFile C:\\Users\\your_user_name\\.ssh\\id_rsa_gitlab
PreferredAuthentications publickey
User gitlab_username
```

**6. 测试**

命令行测试连通性

```
ssh -T git@github.com
ssh -T git@gitlab.com
```

# 配置局部Git账号

由于未在全局变量中配置Git账号信息，需要在项目中配置局部变量。

```shell
# 进入Git项目中，输入如下命令
git config user.name "your_name"
git config user.email "your_email@example.com"
# 如果需要全局Git账号，可以输入如下命令（如果没有局部Git账号则会用全局Git账号）
git config --global user.name "your_name"
git config --global user.email "your_email@example.com"

```
