---
title: MVVM-Sidekick入门(1)---CommandModel
date: 2016-02-16 14:45:12
tags: 
- MVVM-Sidekick
- MVVM
- WPF
- C#
categories: 
- MVVM-Sidekick
---

## CommandModel简介

在ViewModel 中使用 `propcmd6 [Tab] [Tab]` （如果不用 C#6 就是 `propcmd` ）命令会出现如下代码片段：

``` CSharp
public CommandModel<ReactiveCommand, String> CommandSomeCommand
{
    get { return _CommandSomeCommandLocator(this).Value; }
    set { _CommandSomeCommandLocator(this).SetValueAndTryNotify(value); }
}
#region Property CommandModel<ReactiveCommand, String> CommandSomeCommand Setup        

protected Property<CommandModel<ReactiveCommand, String>> _CommandSomeCommand = new Property<CommandModel<ReactiveCommand, String>> { LocatorFunc = _CommandSomeCommandLocator };
static Func<BindableBase, ValueContainer<CommandModel<ReactiveCommand, String>>> _CommandSomeCommandLocator = RegisterContainerLocator<CommandModel<ReactiveCommand, String>>(nameof(CommandSomeCommand), model => model.Initialize(nameof(CommandSomeCommand), ref model._CommandSomeCommand, ref _CommandSomeCommandLocator, _CommandSomeCommandDefaultValueFactory));
static Func<BindableBase, CommandModel<ReactiveCommand, String>> _CommandSomeCommandDefaultValueFactory =
    model =>
    {
        var resource = nameof(CommandSomeCommand);           // Command resource  
        var commandId = nameof(CommandSomeCommand);
        var vm = CastToCurrentType(model);
        var cmd = new ReactiveCommand(canExecute: true) { ViewModel = model }; //New Command Core

        cmd.DoExecuteUIBusyTask(
                vm,
                async e =>
                {
                    //Todo: Add SomeCommand logic here, or
                    await MVVMSidekick.Utilities.TaskExHelper.Yield();
                })
            .DoNotifyDefaultEventRouter(vm, commandId)
            .Subscribe()
            .DisposeWith(vm);

        var cmdmdl = cmd.CreateCommandModel(resource);

        cmdmdl.ListenToIsUIBusy(
            model: vm,
            canExecuteWhenBusy: false);
        return cmdmdl;
    };

#endregion
```

初次接触的人会感觉乱七八糟的，但其实还是比较有条理的，让我们一步一步来解释，首先命令的所有配置都在 `_CommandSomeCommandDefaultValueFactory` 中完成。

``` CSharp
var resource = nameof(CommandSomeCommand);           // Command resource  
var commandId = nameof(CommandSomeCommand);
var vm = CastToCurrentType(model);
var cmd = new ReactiveCommand(canExecute: true) { ViewModel = model }; //New Command Core
```

首先，先建立一个ReactiveCommand，这里canExcute，就是命令在最开始的时候是否可以被执行。resource是命令的一个资源，可以往里面放任何东西，比如说按钮显示的文本之类的。vm是当前的ViewModel的实例。

然后就是配置这个Command的任务了，我们可以通过两种方式来向命令添加任务

1. cmd.DoExecuteUITask
2. cmd.DoExecuteUIBusyTask

每个vm都有一个IsUIBusy属性来标识当前UI是否处于Busy状态，默认的DoExecuteUIBusyTask方法是向页面通知这个方法比较费时，页面可以显示一个Loading。但是如果在导航的时候使用DoExecuteUIBusyTask，返回的时候会导致原页面还处于Busy状态，导致command无法继续执行，因此把DoExecuteUIBusyTask改为DoExecuteUITask就可以了。

> 这里参考了[yan_xiaodi的博客](http://www.cnblogs.com/yanxiaodi/p/3802717.html)

后面的 `.DisposeWith(vm);` 要注意（特别是导航类的程序），这表明在ViewModel释放时这些会被销毁，具体在[ViewModel一节](/2016/02/16/MVVM-Sidekick入门-3-ViewModel/)中具体说明。

接下来配置canExcute，这里用如下代码进行配置：
``` CSharp
cmd.ListenCanExecuteObservable(/*Some Observable*/).DisposeWith(vm);
```
如果要订阅多个事件流，应该利用Rx将多个事件流合并。

> 注：此时如果行为与你预期不符，应该删掉后面ListenToIsUIBusy。两者都订阅的话会因为其中一个事件流变化开始或者关闭。如果你需要两个同时订阅的话，应该这样写：
> ``` CSharp
cmd.ListenCanExecuteObservable(
                    vm.ListenChanged(x => x.IsUIBusy, x => x.IsLoading)
                        .Select(_ => !vm.IsUIBusy && !vm.IsLoading));
```

然后是否要监听UIBusy，并在canExecuteWhenBusy参数中选择在UIBusy时命令是否可以执行：
``` CSharp
cmdmdl.ListenToIsUIBusy(
            model: vm,
            canExecuteWhenBusy: false);
```

## 异常处理
如果我们要处理命令中发生的异常，应该这么做：
``` CSharp
MVVMSidekick.EventRouting.EventRouter
    .Instance
    .GetEventChannel<Task>().Subscribe(o =>
{
    //To Do
});
```
这里 `o.EventData` 就是命令的Task，比如判断是否错误就判断 `IsFaulted` 属性。

比如说一条语句 `throw new Exception();` 的命令 `CommandTest` ，里我们会得到如下结果：
![o的数据](MVVM-Sidekick入门-1--CommandModel/o.png)

> 作者注：这里是通用处理的位置，任何用ExecuteUITask/ExecuteTask 的内容都可以用这种方式监听，但是我们不排斥直接在Task逻辑内进行TryCatch.

## 使用时可能遇到的其他问题

如果后台直接 `CommandSomeCommand.Excute(null)` 可能不会执行，原因是有 `ListenToIsUIBusy` ，当IsUIBusy为true时命令是无法被执行的，将canExecuteWhenBusy改为true会修复这个问题。（但是一般不应在后台Excute Command）