---
title: vim 学习笔记
date: 2020年7月6日
comments: true
categories: linux
tags:
- vim
---

## vim 学习笔记

### 基本操作

正常模式下进入插入模式几种方式：

a光标后插入，i当前光标插入，I行首插入，A行尾插入，o下行插入，O上行插入，s删除当前光标插入，S删除整行插入

1. 替换

<!-- more -->

:% s/需要替换的字符/替换后的字符/g(全局替换)

r 替换当前光标

R 替换模式

2. 删除

instrt 模式下：

ctrl+h 删除上一个字符

ctrl+u 删除整行

ctrl+w 删除上一个单词

正常模式可视化模式下：

x 删除当前字符

dd 删除整行

## 快速切换 normal 和insert模式

CTRL+ [ 退出insert模式
gi 由normal模式进入最后编辑的位置

## 插件管理

使用`vim-plug`进行插件管理

安装

```bash
curl -fLo ~/.var/app/io.neovim.nvim/data/nvim/site/autoload/plug.vim \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

## 键位映射

| &lt;k0&gt; - &lt;k9&gt; | 小键盘 0 到 9       |
| ----------------------- | --------------- |
| &lt;S-...&gt;           | Shift＋键         |
| &lt;C-...&gt;           | Control＋键       |
| &lt;M-...&gt;           | Alt＋键 或 meta＋键  |
| &lt;A-...&gt;           | 同 &lt;M-...&gt; |
| &lt;Esc&gt;             | Escape 键        |
| &lt;Space&gt;           | 插入空格            |
| &lt;Tab&gt;             | 插入Tab           |
| &lt;CR&gt;              | 等于&lt;Enter&gt; |
| &lt;Up&gt;              | 光标上移            |
| &lt;Left&gt;            | 光标左移            |
| &lt;Down&gt;            | 光标下移            |
| &lt;Right&gt;           | 光标右移            |

## 我的vim配置

[GitHub - Fxuing/vimrc: vimrc](https://github.com/Fxuing/vimrc)
