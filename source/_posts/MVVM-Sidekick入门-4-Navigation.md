---
title: MVVM-Sidekick入门(4)---Navigation
date: 2016-02-16 14:45:48
tags: 
- MVVM-Sidekick
- MVVM
- WPF
- C#
categories: 
- MVVM-Sidekick
---

WPF通过StageManager来实现，如果不加Frame，导航请用UserControl（Window会新开一个窗口，Page会需要改变窗口大小后才能导航）。
新建Window、UserControl、Page时请确保用项目模板创建
有三个方法可供使用

1. Show
2. ShowAndGetViewModelImmediately
3. ShowAndReturnResult

如果不传进去第一个参数，那么就会使用ViewModel的默认值
如果使用Show和WaitForCloseWithResult都会等到对应的视图关闭时才会返回，ShowAndGetViewModelImmediately则会立即返回
ShowAndGetViewModelImmediately会立即返回一个Closing任务和一个ViewModel。可以接着await

一个最简单的显示可以这样写：
``` CSharp
await vm.StageManager.DefaultStage.Show<Control1_Model>();
```
如果要导航一般用
```CSharp
await vm.StageManager.DefaultStage.ShowAndGetViewModelImmediately<Control1_Model>();
```
导航时会运行OnBindedViewLoad方法（包括启动Window时）