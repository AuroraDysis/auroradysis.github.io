---
title: MVVM-Sidekick入门(8)---Behavior
date: 2016-02-16 14:46:17
tags: 
- MVVM-Sidekick
- MVVM
- WPF
- C#
categories: 
- MVVM-Sidekick
---

暂时只有三个Behaviors
- BaeconBehavior
- ListenToEventRouterDataBehavior
- SendToEventRouterAction

1. 添加Interactions库的引用。主要添加如下两个dll：
Microsoft.Expression.Interactions.dll和System.Windows.Interactivity.dll。
2. 使用代码

``` Xml
xmlns:i="http://schemas.microsoft.com/expression/2010/interactivity"
xmlns:ei="http://schemas.microsoft.com/expression/2010/interactions"
xmlns:behaviours="clr-namespace:MVVMSidekick.Behaviors;assembly=MVVMSidekick"

<i:Interaction.Behaviors>
    <behaviours:ListenToEventRouterDataBehavior/>
    <behaviours:BaeconBehavior/>
</i:Interaction.Behaviors>
<i:EventTrigger>
    <behaviours:SendToEventRouterAction/>
</i:EventTrigger>
```

## ListenToEventRouterDataBehavior

该Behavior有以下属性
LastDataReceived，最后一个收到的数据
EventObjectType，事件数据类型，决定所选Channel
现在还是：targetEventRouter.GetEventChannel<object>()//ListenToEventRouterTrigger.cs——298行
即只会监听object通道，如果要raise事件还要IsEventFiringToAllBaseClassesChannels = true;
EventRouter，如果该属性不设置的话就会默认使用EventRouter.Instance
EventRoutingName，用来筛选EventRouter中的事件名称，如果是空就是所有事件

## BaeconBehavior

该Behavior有依赖项属性BaeconName
该依赖项属性与StageManager.BeaconProperty附加属性绑定，最终结果是让一个ContentControl像一个Stage一样工作
所以当然必须附加到一个ContentControl上

样例：
    
``` Xml
<ContentControl>
	<i:Interaction.Behaviors>
        <behaviours:BaeconBehavior BaeconName="RootStage"/>
    </i:Interaction.Behaviors>
</ContentControl>
```

``` CSharp
await vm.StageManager["RootStage"].Show<SettingView_Model>();
```
	
## SendToEventRouterAction

该Action有以下属性,具体和ListenToEventRouterDataBehavior差不多，就是一个是发送，一个是接收，不再赘述
EventRoutingNameEventRouter
EventDataType
IsEventFiringToAllBaseClassesChannels
IsEventFiringToAllImplementedInterfacesChannels
EventData
