# 如何在 React 中使用窗口事件并访问状态

> 原文：<https://javascript.plainenglish.io/how-to-use-window-events-in-react-and-access-current-state-inside-callbacks-1958816fbdb7?source=collection_archive---------1----------------------->

![](img/6b1538296893eccb2a48a1a66ca435f2.png)

## 使用窗口事件和在 react 钩子中访问组件当前状态的指南

如果您曾经尝试在 React 中使用窗口事件，您会发现事件中组件的状态已经过时，因此下面是组件应该是什么样子才能工作，以及为什么会发生这种情况的解释。

这是我们的空组件和一些导入，现在我们必须让它工作。

现在，我们需要添加一些钩子，***【useLayoutEffect】****类似于***componentDidMount***(它们都在组件的同一相位上发射，在这里阅读更多关于这个钩子的内容[](https://reactjs.org/docs/hooks-reference.html#uselayouteffect)**)***

***在下面的代码中，我添加了带有回调函数的***useLayoutEffect***，它返回一个清理函数。***

***接下来，我创建了一个 **useState** 钩子、一个 **useRef** 钩子和一个函数。***

***但是为什么呢？当我们向事件监听器发送回调时，它会获取当前状态的一个“*快照*”，因此在下一个触发事件中，它将使用钩子被调用时的初始状态，而不是当前状态，这就是为什么我们需要使用该值作为 ref。***

***这是我们最后的代码，解释在那下面。***

***在前 13 行中，我们定义了我们的状态；有三个部分。首先，我们像往常一样设置我们的 ***useState*** 钩子，但是有一点变化，在第二个参数的开头有一个下划线。
然后我们创建一个 ref，默认参数为 ***useState*** 值。现在我们有了国家和裁判。
最后，我们使用 ***setState*** 的命名约定创建一个函数(与第一个类似，但没有下划线)。它做两件事，设置 ref 值和设置 state 值。所以当我们调用我们的 ***setState*** 的时候；它更新状态和 ref。状态在组件中使用，引用在事件回调中使用。***

***接下来的部分是 ***useLayoutEffect*** 。我们为回调创建一个函数(不是一个箭头函数，以便清理工作)，然后我们定义事件并传递这个回调。
如你所知，效果钩子可以返回一个清理函数(类似于***component will unmount***如果你来自基于类的)。我们移除该函数中的事件监听器，传递回调函数。
在回调里面，我们可以使用 ***ref.current*** ，它会有当前状态，你也可以使用我们上面创建的 ***setState*** 函数。***

***如果你想玩代码，我会留下一个沙盒。***

## ***希望这篇文章对你有用，感谢阅读。💕😁***