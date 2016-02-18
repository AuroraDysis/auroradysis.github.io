---
title: MVVM-Sidekick入门(7)---ServiceLocator
date: 2016-02-16 14:46:10
tags: 
- MVVM-Sidekick
- MVVM
- WPF
- C#
categories: 
- MVVM-Sidekick
---

本框架集成Unity，通过MVVMSidekick.Services.ServiceLocator(还有一个同名类在Microsoft.Practices.ServiceLocation下)这个抽象类可以设定和访问Instance，默认是UnityServiceLocator，通过SetInstance方法可以改变默认实例，需要实现IServiceLocator接口

其他就是Unity框架的问题了，但是要注意有的接口有所改变，比如RegisterType并注册为单例需要这样使用：
ServiceLocator.Instance.Register<IAccountService>(new AccountService());
而不是再使用ContainerControlledLifetimeManager

## 直接使用UnityContainer

``` CSharp
var container = ((MVVMSidekick.Services.UnityServiceLocator) ServiceLocator.Instance).UnityContainer;
```