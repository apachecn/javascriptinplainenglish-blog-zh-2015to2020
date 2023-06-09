# 抓住 RxJs:UI 组件中的可观察对象 vs 主体 vs 行为主体

> 原文：<https://javascript.plainenglish.io/grasp-rxjs-observable-vs-subject-vs-behaviorsubject-in-ui-components-d34b95761814?source=collection_archive---------7----------------------->

![](img/25c483947c2db8a27a6a5574c02ee57f.png)

Photo by [Blake Weyland](https://unsplash.com/@blakeweyland?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当习惯于 RxJs 时，理解使用哪种风格的 Observable 有时会有点棘手。今天我们将专注于 UI 组件，以及你应该为哪种行为使用哪种风格。

请注意，虽然还有其他风格的 observable，如 RelaySubject、AsyncSubject 和 ReplaySubject，但我发现 Observable、Subject 和 BehaviorSubject 几乎构成了 UI 组件中使用的所有可观察流，所以我将重点介绍这三种。

让我们先简单介绍一下每种类型。因为我们在这里着眼于实际应用，所以我们不会深入研究它们是如何工作的。已经有许多文章深入解释了它们的行为。

# 介绍口味

## 可观察量

可观察类型是 RxJs 中可用的可观察流中最简单的一种。将它与主体及其子类型区分开来的是这样一个事实:可观察物通常是由一个创造函数创造出来的，如`of`、`range`、`interval`等。或者在已经存在的可观察流上使用`.pipe()`。虽然`new Observable()` 也是可能的，但我发现它不太常用。

另一个重要的区别是，Observable 不像 Subject 那样公开`.next()`函数。`.next()`允许以参数`next`为值手动触发发射。

这意味着开发者通常可以通过查看它的声明来看到一个可观察对象的所有可能的发射。本质上没有“隐藏的”排放，相反，只要看看它是如何产生的，就可以对整个潜在排放进行审查。

请注意，当管道连接到对象时，通常会产生可观测量，在这种情况下，理解来源的排放并不简单，但是如果您以“假设来源排放……”开始推理，您可以通过例如查看管道中的运算符来推理给定可观测量中所有可能的排放。

## 科目

主体扩展了可观察范围，但行为完全不同。主题是使用`new Subject()`创建的，声明中完全没有提到它可能会或不会发出什么。如果您使用 TypeScript(您希望这样做),您可以推断出发射的类型，但是没有办法通过简单地查看它的声明来推断出它将在何时以及在什么情况下发射。

原因是受试者暴露了`.next()`，这是一种手动推送排放的方法。这意味着您可以通过编程方式声明其排放量。这是一个非常强大的功能，同时也很容易被滥用。

一般来说，如果您订阅了一个可观察到的，以某个东西被推给一个主题结束的。`next()`，你可能做错了什么。通常，您只想将可观测值写成纯反应。

受试者在创作时不持有任何排放权，并且依赖`.next()`进行排放。因此，默认情况下，任何主题上的订阅都将异步运行。

## 行为主体

行为主体是主体的另一种风格，它改变了一件(非常)重要的事情:它将最新的排放保持在内部状态变量中。与此相关的是，行为主体的两个方面与主体的行为有点不同:

1.  当使用新的行为主体时，它在创建时需要一个初始值，这意味着内部状态变量永远不能以某种方式声明
2.  它公开了`.getValue()`，一个同步返回内部状态变量的方法

因此，每当对行为主体调用`.next()`时，它会做两件事:用输入覆盖内部保存的变量，并将该值发送给订阅者。

这也意味着行为主体上的任何订阅都会立即以同步方式接收内部保存的变量作为发射。任何随后的发射异步进行，就像它是一个常规受试者一样。

这些都是非常显著的差异！这意味着，例如，如果您在 BehaviorSubject 上使用带有`.take(1)`的订阅，当管道被激活时，您保证会有同步的反应。这也意味着您可以使用`.getValue()`在管道和订阅之外的任何地方同步获取当前值。

这使得行为主体成为数据持有人的自然选择，无论是对于反应流还是更普通的 javascript 过程。

# 哪种味道在什么情况下

因此，基于对这些行为的理解，您应该在什么时候使用它们？

**可观察**应该用在你写纯反应的时候。这些应该只是描述当某些事件发生时你想发生什么。

**受试者应以**为信号源。对于 UI 组件来说尤其如此，其中一个按钮可以有一个侦听器，该侦听器只需在处理输入事件的主题上调用`.next()`。

**行为主题**应在组件中使用内部状态时使用，用于在初始化和反应时也需要反应的数据字段。

举个简单的例子，假设我们有一个同意页面，页面上有一个包含三个元素的文本框:

1.  在用户可以访问下一页之前，必须滚动到底部的同意说明框
2.  需要用户键入 *yes* 才能访问下一页的文本输入
3.  当用户无法访问下一页时，用于访问下一页的按钮应被禁用

使用各种各样的可观测量来解决这个问题的一种方法如下:

*   `userInput$: BehaviorSubject<string>`保存当前用户输入
*   当用户滚动到文本框底部时发出 true。
*   `hasGivenConsent$: Observable<boolean>`通过`map`发送`userInput$`，当发送值为*是*时，返回`true`，否则返回`false`
*   `canProceed$: Observable<boolean>`使用`hasScrolledText$`和`hasGivenConsent$`上的`combineLatest`，如果两个输入发射都是`true`，则映射到`true`，否则映射到`false`。

最后，下一页按钮的`disabled`字段订阅与`canProceed$`相反的内容。

今天的文章在技术上可能没有以前的文章那么详细，老实说，更多的是我对在 UI 组件中使用 Observables 的最佳实践的看法。由于这个话题更多的是基于观点而不是事实，我很乐意看到任何对我的观点有争议的评论！

和往常一样，保持你的成功之路！