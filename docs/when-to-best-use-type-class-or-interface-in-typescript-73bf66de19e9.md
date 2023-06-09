# 什么时候应该在 TypeScript 中使用类型、类或接口？

> 原文：<https://javascript.plainenglish.io/when-to-best-use-type-class-or-interface-in-typescript-73bf66de19e9?source=collection_archive---------1----------------------->

![](img/6ae06ab66cdcf964011974928563f73d.png)

Photo by [Katrin Hauf](https://unsplash.com/@trine?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/typewriter?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当我刚开始学习 TypeScript 时，我纠结于这些概念，仍然找不到任何关于该主题的有用文章。我相信许多人可能对这个主题感到好奇，那么为什么不写一写呢？

首先，你必须明白这个话题是有争议的。甚至 TypeScript 文档也很含糊，或者省略了对这些不同数据结构的深入比较。

我找到的大多数信息都是关于它 ***是什么*** 而不是如何 ***高效地选择*** 正确的数据类型。

# 先决条件。

理解本文不需要太多的 TypeScript 知识。这个想法是为了让您对这些数据结构的作用有一个大致的了解。加上一些容易记住的经验法则，你将会很好地掌握这个中级话题。

# 理解值空间和类型空间。

这三种数据结构可以存在于同一个平面中，尽管两种结构在运行时甚至不存在。理解这个概念可能会让你对使用打字有一个新的看法。如果您来自其他静态类型语言，这个例子将对您有很大帮助:

Here’s an example

这是不会引发错误的有效代码。为什么会这样呢？

在 TypeScript 中，类型和值不存在于同一命名空间中。这两个标记没有关系，它们不会冲突，因为 TypeScript 是 JavaScript 的超集。它可以帮助您在试图将自己编译成优秀的旧 JavaScript 时发现错误。因此，类型`Cat`在运行时将不存在。生成的代码可以在浏览器中运行，或者在其他情况下，在一个准系统节点进程中运行。表达式`type Cat`在普通 JavaScript 中无效；因此，正如你所猜测的，它删除了它。

在这一点上，你可能对类感到疑惑吗？你意识到类在现代 JavaScript 中是有效的，这是对的。但是你必须明白，你也可以使用类作为类型注释。

Whoops, naming collision!

这就对了，TS 中的类存在于值和类型空间中。在这个特殊的例子中，我们只是告诉 TS，`dog`将是一个类型为`Dog`的对象。

*   类存在于类型空间和值空间中，因为它们在编译后仍然存在，并且可以用于在运行时创建新的对象。
*   类型只存在于命名空间中；
*   接口只存在于命名空间中。

现在类型和接口的意义是什么——我的意思是，一切都可以是一个类，对吗？嗯，是也不是。

TypeScript 是一种工具，它可以帮助您发现语法错误，记录在函数中传递的数据，并且通常比纯 JavaScript 提供更好的开发体验。

在你的。ts 文件，是虚构的土地。你认为你是在用一种新的语言编写，但实际上，它只是在你试图运行代码之前，为编译器添加注释以发现你的错误。

对这个概念有了更清晰的理解，让我们继续讨论你为什么会在这里！

# 什么时候应该在 TypeScript 中使用类型？

与类不同，类型不表达应用程序内部的功能或逻辑。当你想描述某种形式的信息时，最好使用类型。它们可以描述各种形状的数据，从字符串、数组和对象等简单结构。

*   你想表达某种形式的信息，你可以特别命名(例如:`SquareType`代表一个正方形)。
*   帮助 TypeScript 调试代码，或者在使用 IDE 帮助进行编码时防止错误。
*   作为简洁的函数参数来传递。
*   描述一个类的构造函数参数。
*   记录从 *API 的*或 *SDK 的*进出的大型对象。
*   它们在小的时候工作得最好。
*   通常，它们既不持有状态也不持有方法；
*   当你想表达一个值可以是两种不同的类型时，这就叫做并集——应该可以有区别地命名这个并集(例如:`Cat`或`Dog`并集是`AnimalType`)。
*   如果你表达了太多的关联，你可能想用一个[枚举](https://www.typescriptlang.org/docs/handbook/enums.html)来代替。

Some simple type examples

# 什么时候应该在 TypeScript 中使用接口

大多数从 TypeScript 开始的人感到困惑的地方是类型和接口之间的区别是什么？随着这种语言的最新版本，它们变得越来越相似。

您可能希望在类型上使用接口只有两个原因，其中一个是声明合并，另一个是编码风格(或偏好)问题。在你开始在评论中对我大喊大叫之前，请继续阅读。

Declaration merging, baby.

这是完全有效的代码！如果您要使用类型、扩展或合并，您将不得不经历创建连接两者的新类型的麻烦。

当您编写由第三方应用程序使用的库时，这非常有用。一个很好的例子就是 [express.js](https://www.npmjs.com/package/express) ，这是一个简单而强大的包，可以帮助你为你的应用程序创建路线、控制器和中间件。

Get rid of your types folder!

非常方便！现在您将拥有 IDE 自动完成功能，当您稍后回到这条路线时，理解 req 对象中发生的事情将会容易得多。

> *注意:每次我向那些在项目结构中创建自定义类型文件夹来解决这个问题的开发人员展示这一点时，他们的大脑都会爆炸！你已经被警告了。*

您可能希望使用接口的另一个原因纯粹是从编码风格的角度出发。这个想法是你也可以对类型做下面的事情——但是出于某种原因，我觉得这样做更明显。

假设您有一个包含两种(或多种)类的动物数组。为了简单起见，我们就说`Cat`和`Dog`。在循环数组和访问属性时，应该确保这两个类有一些共同点。

Is cat at dog? Good question!

接口不像`extend`那样在类中引入方法或属性。它只告诉 TS 编译器猫和狗都应该有`isDog()`方法。当你写你的类实现时，如果你遗漏了什么，它会提醒你。

这个例子非常简单；然而，在与大型团队合作的大型项目中，这是天赐良机。接口变成了某种契约，必须由实现它的类来履行。现在，与“扩展”另一个类的最大区别是，在狗和猫的案例中，`isDog()`的内部逻辑可能不同——然而，它们的结果是相同的。在这种情况下，它们都应该返回一个布尔值。

喜欢界面就叫我潮人吧。我爱他们。

# 什么时候应该在 TypeScript 中使用类

本质上，对大多数人来说，类在使用上比类型或接口更简单。

类是大多数* TypeScript 项目的基础。它们定义了一个物体的蓝图。它们表达了这些对象将继承的逻辑、方法和属性。

在 JS 或 TS 中，类不会在你的应用程序中创建新的数据类型；您可以将它用作对象工厂。当您传递用`new`关键字实例化的类时，您只是在它实现的对象周围移动。

您可以在各种范例中使用一个类；您可以扩展它们，组合它们，在其他类中注入它们的依赖关系。这取决于你为你的应用找出最好的结构，并保持与该风格一致。然而，这可以是另一篇文章的主题！

*   如果需要随时间改变对象的状态；
*   如果您觉得该对象需要方法来查询或改变其状态；
*   当您希望将行为与数据更紧密地关联起来时；
*   当你需要为你的应用程序编写功能逻辑时。
*   如果你只在类中写一些属性赋值，你可以考虑用类型来代替。

GoodBoyEvaluator is a very popular framework.

# 对你有帮助吗？

我写了一篇我希望三年前就能找到的文章。起初，它们可能是很难理解的复杂概念——不要让这吓倒你。学习 TypeScript 绝对值得努力。一段时间以来，使用它是我最大的(可以说是最大的)生产力提升。

如果你仅仅来自 JavaScript，它将帮助你理解其他类型语言是如何工作的；这是一种入门药物。

如果有什么可以说得更清楚的，请在评论中告诉我！感谢您的阅读！