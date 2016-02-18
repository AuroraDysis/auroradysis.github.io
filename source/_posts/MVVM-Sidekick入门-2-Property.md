---
title: MVVM-Sidekick入门(2)---Property
date: 2016-02-16 14:45:31
tags: 
- MVVM-Sidekick
- MVVM
- WPF
- C#
categories: 
- MVVM-Sidekick
---

## Property简介

在ViewModel 中使用 `propvm6 [Tab] [Tab]` （如果不用 C#6 就是 `propvm` ）命令会出现如下代码片段：
``` CSharp
public int MyProperty
{
    get { return _MyPropertyLocator(this).Value; }
    set { _MyPropertyLocator(this).SetValueAndTryNotify(value); }
}
#region Property int MyProperty Setup        
protected Property<int> _MyProperty = new Property<int> { LocatorFunc = _MyPropertyLocator };
static Func<BindableBase, ValueContainer<int>> _MyPropertyLocator = RegisterContainerLocator<int>(nameof(MyProperty), model => model.Initialize(nameof(MyProperty), ref model._MyProperty, ref _MyPropertyLocator, _MyPropertyDefaultValueFactory));
static Func<int> _MyPropertyDefaultValueFactory = () => default(int);
#endregion
```

初始值在 `_MyPropertyDefaultValueFactory` 中设定，即将 `default(int)` 替换成需要的初始值（可以是运算表达式）。

如果初始值比较复杂的话使用 `propcvm6 [Tab] [Tab]` （如果不用C#6就是 `propcvm6` ）命令产生片段：

``` CSharp
public int MyProperty
{
    get { return _MyPropertyLocator(this).Value; }
    set { _MyPropertyLocator(this).SetValueAndTryNotify(value); }
}
#region Property int MyProperty Setup        
protected Property<int> _MyProperty = new Property<int> { LocatorFunc = _MyPropertyLocator };
static Func<BindableBase, ValueContainer<int>> _MyPropertyLocator = RegisterContainerLocator<int>("MyProperty", model => model.Initialize("MyProperty", ref model._MyProperty, ref _MyPropertyLocator, _MyPropertyDefaultValueFactory));
static Func<BindableBase, int> _MyPropertyDefaultValueFactory =
    model =>
    {
        var vm = CastToCurrentType(model);
        //TODO: Add the logic that produce default value from vm current status.
        return default(int);
    };
#endregion
```
