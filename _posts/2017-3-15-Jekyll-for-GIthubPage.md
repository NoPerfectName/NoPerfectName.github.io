---
layout: post
title: "使用Jekyll和Github page搭建个人博客"
categories: Jekyll
tags: Jekyll
author: NoPerfectName
---

* content
{:toc}

利用Jekyll和Github pages搭建个人博客




以下内容针对的是在Ubuntu环境下的配置。
<br/>
## 事前准备

#### Github的准备
* 一个Github账户
* 一个与用户名同名的仓库，并将其设置为站点。
* Git的本地配置

具体可参见官网[Github Pages](https://pages.github.com/)

#### Jekyll的准备
* 安装Ruby
> $ sudo apt-get install ruby-full
* 安装RubyGems
> 进入[RubyGems官网](https://rubygems.org/pages/download) 

* 安装Jekyll
> gem install jekyll

具体可参见官网[Jekyll官网](http://jekyll.com.cn/docs/installation/)
<br/>
## 创建博客
* 创建本地仓库，并从远程拷贝
> git clone *'github地址'*
* [Jekyll theme](http://jekyllthemes.org)下载喜欢的主题
* 根据主题的安装提示安装
* 预览效果
> jekyll s

* 上传到Github
> git status     //查看状态
> git add .      //将修改的文件从工作区添加到暂存区
> git commit -m "备注"      //把暂存区的内容提交到分支
> git push origin master     //上传到github上
