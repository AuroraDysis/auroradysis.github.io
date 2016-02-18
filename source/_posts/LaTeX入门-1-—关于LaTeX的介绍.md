---
title: LaTeX入门(1)—关于LaTeX的介绍
date: 2016-02-16 18:27:11
tags:
- LaTeX
categories: 
- LaTeX
---

$\LaTeX$来自于$\TeX$（Knuth开发），$\TeX$有300多个原语可以完成所有的排版任务，但是写出来的东西晦涩难懂。所以后来加了600多个宏，变成了$plain-\TeX$的格式。变得好理解不少了，Leslie Lamport感觉$plain-\TeX$还是很不方便就取了自己名字的前两个字母于1980年代初开发出了$\LaTeX$（在$\TeX$上做了一层封装），1992年发布$\LaTeX$ 2.09以后就撒手不管，2001去了微软（13年拿了图灵奖）。


## LaTeX引擎
 **引擎翻译源码**
 下面引擎大致按时间排序

- $\TeX$
- $\varepsilon-\TeX$
- $pdf\TeX$

**从这里开始支持Unicode编码，可以读取系统中的字体**

- $Xe\LaTeX$
- $Lua\TeX$
- $Omega$        //支持16种文字方向
- $p\TeX$        //日本开发
- $up\TeX$
- $p\TeX-ng$        //ng代表next generation，中国人开发

## 格式——代码风格
一个格式对应的代码就要用对应的工具来编译

- $plain \TeX$
- $\LaTeX$
- $Con\TeX t$        //编程功能强一些，能支持更复杂的板式

## 宏包——功能
宏包对应某一功能，如同编程中的类库，下面是常用的宏包
- graphicx--插图
- tabu--插入表格
- hyperref--超链接宏包
- fancyhdr--做页眉页脚
- titlesec--章节标题样式
- natbib--用来调整参考文献格式

## 辅助工具
- BiBTeX--参考文献
- makeindex--索引
- 编辑器
- **TeXworks**
- MiKTeX和TeX Live默认编辑器，功能简单，适合新手，以TeXshop为原版设计出来
- **TeXshop**
- MacTeX的默认编辑器，功能简单，适合新手
- 其他还有**TeXmaker, TeXstudio, TeXnicCenter, WinEdt, Sublime Text, LyX, Vim, Emacs, ShareLaTeX**等
对比具体见http://www.zhihu.com/question/19954023

## 常见发行版（发行版即上述工具的集合）

- **MiKTeX**
Windows，32位、64位，安装包小（去掉了不常用的宏包，可以自己检测下载需要的宏包），开发者少更新慢

- **CTeX**
是个人作品，基于MiKTeX，分为基本版和完整版（建议完整版），封装WinEdt（商业软件，Windows上最强大）

- **TeX Live**
由TUG维护，全平台支持，安装方式很多，开发者多且水平高，BUG少

安装流程见：http://tieba.baidu.com/p/3524644809

- **MacTeX**
TeX Live专门为Mac OS X系统优化的版本

- **W32TeX**
基于TeX Live,Win32
http://w32tex.org/index-zh.html
