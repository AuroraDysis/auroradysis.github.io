---
title: MVVM-Sidekick入门(5)---EventRouter
date: 2016-02-16 14:45:56
tags: 
- MVVM-Sidekick
- MVVM
- WPF
- C#
categories: 
- MVVM-Sidekick
---

MVVMSidekick.EventRouting名称空间中有着EventRouter和EventRouterHelper

EventRouter提供默认单例，也可以自行创建。
EventRouter具有一个静态方法，通过EventRouter默认实例发送数据：

``` CSharp
public static void RaiseErrorEvent<TException>(object sender, TException exception, [CallerMemberName] string callerMemberOrEventName = null) where TException : Exception
```

所有EventChannels放在protected readonly ConcurrentDictionary<Type, IEventChannel>中，线程安全。

EventRouter具有如下方法，支持泛型和非泛型，功能和参数和名称一致：
isFiringToAllBaseClassChannels 是否在基类频道Raise
isFiringToAllImplementedInterfaceChannels 是否在所有隐式接口频道Raise

``` CSharp
public virtual void RaiseEvent<TEventArgs>
(object sender, TEventArgs eventArgs, string callerMemberNameOrEventName = "", bool isFiringToAllBaseClassChannels = false, bool isFiringToAllImplementedInterfaceChannels = false)

public virtual void RaiseEvent
(object sender, object args, Type eventArgsType, string callerMemberNameOrEventName = "", bool isFiringToAllBaseClassChannels = false, bool isFiringToAllImplementedInterfaceChannels = false)

public virtual EventChannel<TEventData> GetEventChannel<TEventData>()
```

EventRouterHelper是一个静态类，仅有一个方法，通过EventRouter默认实例发送数据：

``` CSharp
public static void RaiseEvent<TEventArgs>(this BindableBase source, TEventArgs eventArgs, string callerMemberName = "");
```