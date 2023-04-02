# 编写简洁 JavaScript 的 10 个技巧

> 原文：<https://javascript.plainenglish.io/clean-javascript-10-tips-4c775c02ba0d?source=collection_archive---------7----------------------->

![](img/c64ce2e5b8d229aa138d60b071a539f1.png)

我们都经历过。我们看着一周、一个月、一年前的 JavaScript，想知道最初写它的时候我们在喝什么样的咖啡。🤷‍♂️
很多时候，这取决于三件事的混合:完成工作的可用时间、旧的最佳实践或新的模式以及编写代码的原则。

然而，我们可以做一些事情，这些事情是经得起时间考验的，并将帮助任何来到我们代码库的人，无论是未来的我们还是入职的初级开发人员。我在下面列出了 10 个技巧，我喜欢在编写 JavaScript 时使用它们来保持简洁易读。

# 复杂条件句？`array.some()`去营救

好，我们有一个 if 语句，它非常冗长。很多因素取决于我们是否应该执行一段代码。或者，条件是从我们的应用程序中的其他逻辑动态生成的。像这样的陈述并不少见:

那会变得非常可怕！🤢我们该如何清理呢！？轻松点。数组！

[https://gist . github . com/coly 010/9b 0952 b 88 fc a6 c 6 ee 45 e 2 b 0 BF F2 a4 a 67](https://gist.github.com/Coly010/9b0952b88fca6c6ee45e2b0bff2a4a67)

通过创建一个条件数组，我们可以检查它们中是否有任何一个为真，如果是，那么就执行 if 语句。这也意味着，如果我们需要动态地或者通过一个循环来生成条件，我们可以直接推送到条件数组中。我们也可以很容易地移除定义，只需注释掉`myCondition.push()`或者完全移除它。

*注意:这是创建一个数组并在条件中运行一个循环，因此预期会有一个小的、通常不明显的性能影响*

# ORs 的数组，但是 ANDs 呢？`array.every()`站出来！

几乎和上面的提示一样，除了不只是检查任何一个条件，`array.every()`将检查每个条件都是真的！

就这么简单！

# 没有魔法线

不确定什么是魔弦？它归结为期望输入等于任意字符串值，该字符串值可能代表也可能不代表实现，并可能在其他地方使用，因此使重构变得困难并导致容易出错的代码。
下面是一个神奇琴弦的例子:

正如你从上面的例子中看到的，使用`myString`魔术字符串可以很容易地导致错误的实现。不仅是因为开发人员的拼写错误，而且，如果您通过更改它所期望的神奇字符串来更改`myFunc`，那么调用`myFunc`的所有内容也需要更改，否则它将完全崩溃:

我们可以很容易地解决这个问题，但是创建一个共享对象，用相应的键值设置来定义这些神奇的字符串:

在对象中定义魔术字符串不仅为代码提供了实现上下文，还有助于防止错误拼写和重构带来的错误！💪

# 数组析构返回

我不知道你怎么想，但是确实有很多时候，我希望能够从一个函数中返回不止一个东西，我要么选择返回一个数组，要么选择返回一个包含信息的对象。有一段时间，我倾向于避开返回数组，因为我讨厌看到这样的语法:

对于数组索引`myResult`所代表的内容没有任何上下文，理解这里发生的事情变得有点困难。然而，通过数组析构，我们可以让它更具可读性🤓。看看这个:

这不是让工作变得更容易了吗！？

# 对象析构返回

好吧，数组析构是很棒的，我们可以从中获得一些很好的上下文，但是如果我们只关心函数返回的一些内容，而我们关心的内容与返回的数组不是一个顺序呢？

在这里，返回一个对象可能是一个更好的解决方案，这样我们可以对它执行对象析构:

现在，我们不需要关心项目在返回的数组中的顺序，我们可以安全地忽略我们关心的值之前的任何值🔥

# 多文件与通用文件

即单一责任原则……
好吧，听我说完。使用 bundlers，创建只做一件事的新 JS 文件是非常容易和值得的，而不是用更少的通用文件做很多事情。

如果你有一个名为`models.js`的文件，它包含了定义你的应用程序中所有模型结构的对象，考虑将它们分离到它们自己的文件中！
举这个例子:

一名初级开发人员正在尝试处理与添加 TODO 项目相对应的 API 请求。他们必须进入`models.js`，挖掘 1000 行代码来找到`AddTodoRequest`对象。

一个初级开发人员打开`data-access/todo-requests.js`，看到`AddTodoRequest`在文件的顶部。

我知道我更喜欢哪一个！想想吧。看看你的文件，看看他们是否做得太多了。如果是这样，将代码解压到一个名称更合适的文件中。

# 说出你的黑客

好吧，所以你试图做一些时髦的东西，但没有合适的方法让它工作。也许你必须为特定的浏览器 IE 添加一个解决方法。
您可能确切地知道您用专门用于此变通方法的一段代码做了什么，但是在您之后出现的人可能不知道，甚至是一个月后的您。

帮自己和其他人一个忙，并命名该解决方法！这很容易做到，要么将它放入一个独立的函数中，要么创建一个具有合适名称的局部变量:

现在，任何跟在你后面的人都知道你在做什么！🚀

# 较小的方法

这不言而喻。我知道我们都希望有小方法，但是在现实中，由于时间的限制，这可能说起来容易做起来难。但是，如果我们颠倒一下，如果我们正在编写单元测试，我知道我更愿意为小方法编写单元测试，而不是大方法。

我更希望看到这个:

而不是试图一次完成所有这些独立单元的方法。然后，我们还可以为这些较小的单元编写一些单元测试，并为`myLargeComplexMethod`编写一个非常简单的测试，确保这些较小的单元被正确调用。我们不需要关心它们是否在工作，因为与那些较小的单元相关的单元测试将为我们确保这一点。

# for…of vs forEach

我认为这是不言而喻的，但我们都被回调地狱烧伤了，而`.forEach()`让我想起了太多的回调地狱，甚至不想娱乐它。此外，我们现在已经有了一个很好的方法来循环遍历所有类型的 Iterables，那么为什么不使用它呢？让我们来看看`forEach()`和`for ... of`的对比，你可以自己做决定。

就我个人而言，我更喜欢`for...of`，原因有二:

1.  您可以直接看到其意图是遍历数组中的所有项
2.  对于你的代码库中的任何可重复项，它都是一致的，不管是数组还是映射

`forEach`确实有在回调中提供索引的好处，所以如果这对你有用，那么用那个方法可能更好。

# 移除`try-catch`挡块

最后，我个人的抱怨。`try-catch`街区。我个人觉得它们被过度使用，用错了，它们做了太多，或者捕捉到了它们从未打算捕捉到的错误，这都归结于它们的结构和外观。

我有一个更长的描述为什么我不喜欢他们[这里](https://medium.com/javascript-in-plain-english/lets-clean-up-ugly-try-catches-e4e1a53de500)，但简单来说这里有一个有问题的尝试捕捉:

告诉我你认为这两个都没有问题…如果你的错误处理逻辑很复杂，它只会分散读者对你的方法想要达到的目的的注意力。

我创建了一个小库来解决这个问题:[不要尝试](https://github.com/Coly010/notry)。有了它，我们可以将上述内容转化为:

我个人认为这是一个很好的选择。但那是个人的事！

我希望您能从这篇文章中获得一些有用的技巧，帮助您编写 JavaScript！

如有任何问题，欢迎在下方提问或在 Twitter 上联系我: [@FerryColum](https://twitter.com/FerryColum) 。

## **用简单英语写的 JavaScript 笔记**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[T5T7，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)

我们还推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**