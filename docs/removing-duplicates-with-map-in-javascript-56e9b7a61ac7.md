# 使用 JavaScript 中的 Map 删除重复项

> 原文：<https://javascript.plainenglish.io/removing-duplicates-with-map-in-javascript-56e9b7a61ac7?source=collection_archive---------2----------------------->

![](img/c07fbec6753f30ceb48d810b153ae980.png)

使用`Set`从数组中移除重复项是很常见的。这可以通过将现有数组包装到`Set`构造函数中，然后将其转换回数组来实现:

这对于原始值的数组非常有效，但是当将相同的方法应用于数组或对象的数组时，结果非常令人失望:

这是因为`Set`通过引用而不是通过值来比较非原始值，在我们的例子中，数组中的所有值都有不同的引用。

一个鲜为人知的事实是`Map`数据结构维护了键的惟一性，这意味着不能有多于一个的键-值对具有相同的键。虽然知道这一点不会帮助我们神奇地将任何数组转换成唯一值的数组，但是某些用例可以从 Map 的键唯一性中受益。让我们考虑一个示例 React 应用程序，它显示一个图书列表和一个下拉列表，允许按作者过滤图书。

为了简单起见，`books`数组在这里是硬编码的，尽管在真实世界的应用程序中，数据可能是从 API 中获取的。

该应用程序几乎已经完成，我们只需要呈现作者下拉列表进行筛选。一个好的方法是将我们的图书列表中每个作者的`id`和`name`收集到一个单独的数组中，并将其作为选项呈现在`select`中。然而，有一个条件——这个列表应该只包含唯一的作者，否则不止一本书的作者会多次出现在下拉列表中，这是我们不希望发生的事情。我们需要选项的`value`和`name`的`id`来显示选项的标签，因为作者的数据包含在一个对象中，所以我们不能只应用`Set`技巧来获得唯一值。一种选择是首先将作者的所有`id`放入一个数组，然后对其应用`Set`以获得唯一的一个，之后再次迭代作者数组以基于`id`收集他们的名字。

考虑到我们基本上需要一个由`id` - `name`对组成的数组，我们可以从`books`列表中提取这些对，并将它们转换成一个`Map`，它会自动只保留具有唯一键的对。

就是这样！现在我们有了一个惟一键值对的映射，可以直接输入到 select 组件中。

值得记住的是，当`Map`保持键的惟一性时，最后插入的带有现有键的项留在`Map`中，而之前的重复项被丢弃。

幸运的是，在我们的示例应用程序中，所有的作者`id` - `name`对都是唯一的，所以我们不需要担心意外覆盖任何数据。现在，我们可以将所有内容组合到组件的最终版本中。

【https://claritydev.net】最初发表于[](https://claritydev.net/blog/removing-duplicates-with-map-in-javascript/)**。**