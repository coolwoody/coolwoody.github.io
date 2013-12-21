---
layout: post
title: "octopress在windows7 x64的安装"
date: 2013-12-20 19:53:42 +0800
comments: true
categories: octopress
---
##必备程序安装

octopress 中文内容无法生成提示错误“invalid byte sequence in gbk jekyll”
解决方案：
在git安装目录Git\etc\profile文件最后行里加入以下内容

export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8