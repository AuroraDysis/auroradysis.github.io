---
title: MVVM-Sidekick使用字符串或枚举进行导航
date: 2016-02-16 18:17:08
tags: 
- MVVM-Sidekick
- MVVM
- WPF
- C#
categories: 
- MVVM-Sidekick
---

如果想实现一个存储着各种IViewModel的集合中，通过IViewModel导航到对应ViewModel，最简单的情况这么干

1. 配置Startups，在想要导航的配置文件中修改，分别配置Control1, Control2等等

``` CSharp
public static void ConfigView<TViewModel, TView>(string viewMappingKey)
    where TViewModel : IViewModel
    where TView : class, IView, new()
{
    var viewModel = ServiceLocator.Instance.Resolve<TViewModel>();
    var view = new TView();
 
    var viewMapper = ViewModelLocator<TViewModel>
        .Instance
        .Register(viewModel)
        .GetViewMapper()
        .MapTo<TView>(viewMappingKey, view);
 
    ViewModelLocator<IViewModel>
        .Instance
        .Register(viewMappingKey, viewModel)
        .GetViewMapper()
        .MapTo<TView>(viewMappingKey, view);
}
```

``` CSharp
//Control1.cs
const string key = "Control1";
ConfigView<Control1_Model, Control1>(key);

//Control2.cs
const string key = "Control2";
ConfigView<Control2_Model, Control2>(key);
```

2. 通过ViewMapKey导航到对应View，比如导航到Control1，那就是
 
``` CSharp
//这里可能是已经存储好的
//IViewModel viewModel = ViewModelLocator<Control1_Model>.Instance.Resolve();
await vm.StageManager.DefaultStage.ShowAndGetViewModelImmediately(viewModel, "Control1");

//当然我如果只存一个key那就是
var key = "Control1";
IViewModel viewModel = ViewModelLocator<IViewModel>.Instance.Resolve(key);
await vm.StageManager.DefaultStage.ShowAndGetViewModelImmediately(viewModel, key);
```