---
title: MVVM-Sidekick入门(6)---Startup
date: 2016-02-16 14:46:04
tags: 
- MVVM-Sidekick
- MVVM
- WPF
- C#
categories: 
- MVVM-Sidekick
---

一个一般的Startup是这样的
``` CSharp
ViewModelLocator<Window1_Model>
                .Instance
                .Register(context =>
                    new Window1_Model())
                .GetViewMapper()
                .MapToDefault<Window1>();
                ```

ViewModelLocator本身除了单例实现没有其他东西，其他全部继承自TypeSpecifiedServiceLocatorBase

``` CSharp
public class ViewModelLocator<TViewModel> : TypeSpecifiedServiceLocatorBase<ViewModelLocator<TViewModel>, TViewModel>
                        where TViewModel : IViewModel
```

Register方法有以下重载，都是将一个ServiceLocatorEntryStruct存储到默认Dictionaty里面

``` CSharp
public ServiceLocatorEntryStruct<TService> Register(TService instance)
public ServiceLocatorEntryStruct<TService> Register(string name, TService instance)
public ServiceLocatorEntryStruct<TService> Register(Func<object, TService> factory, bool isAlwaysCreatingNew = true)
public ServiceLocatorEntryStruct<TService> Register(string name, Func<object, TService> factory, bool isAlwaysCreatingNew = true)
public ServiceLocatorEntryStruct<TService> Register(Func<object, Task<TService>> asyncFactory, bool isAlwaysCreatingNew = true)
public ServiceLocatorEntryStruct<TService> Register(string name, Func<object, Task<TService>> asyncFactory, bool isAlwaysCreatingNew = true)
```

还能通过public bool HasInstance(string name = "")来判断是否已经存在实例

ViewModelToViewMapperHelper中定义了

``` CSharp
public static IViewModel GetDefaultViewModel(this IView view);
public static ViewModelToViewMapper<TViewModel> GetViewMapper<TViewModel>(this ServiceLocatorEntryStruct<TViewModel> vmRegisterEntry) where TViewModel : IViewModel;
```

GetDefaultViewModel用来获取当前视图的默认ViewModel，GetViewMapper就是new了一个public struct ViewModelToViewMapper<TModel> where TModel : IViewModel

后面两句代码的作用其实就是将一个View类型和一个ViewModel类型对应起来
