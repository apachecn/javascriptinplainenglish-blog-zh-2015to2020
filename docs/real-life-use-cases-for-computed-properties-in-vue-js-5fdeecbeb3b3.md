# 如何在 Vue 中使用计算属性

> 原文：<https://javascript.plainenglish.io/real-life-use-cases-for-computed-properties-in-vue-js-5fdeecbeb3b3?source=collection_archive---------1----------------------->

![](img/86180de083deda12e1fae140b235ac93.png)

## Vue 文档中的例子并不实用。这里有一些关于如何在 Vue 中编写和使用计算属性的真实例子。

在 Vue 中有多种方法来设置视图的值。这包括直接将数据值绑定到视图，使用简单的表达式或使用过滤器对内容进行简单的转换。除此之外，我们还可以使用计算属性来获取基于数据模型内部项目的值。

Vue 中的计算属性非常有用。以至于除非你想在一个事件之后触发一个函数，比如一个点击事件，否则你应该在查看 methods 属性之前分析你的函数是否可以位于 computed 属性内部。原因是在计算属性内部进行的计算被缓存，并且仅在需要时更新。

既然我们已经给了你什么是计算属性以及它们为什么有用的一个快速概述，那么我们现在最好深入一些例子，这样你就可以更好地了解如何使用它们。当我最初开始学习如何使用 Vue 时，我通读了他们网站上的文档。我发现，虽然它们涵盖了很多主题，但它们并没有深入其中的很多细节。我特别抱怨的是，他们给出的例子通常非常简单，几乎没有用处——比如如何反转一个字符串。因此，让我们通过三个真实的 Vue 计算属性用例来解决这个问题！

你觉得了解这类内容有用吗？如果有，获取更多类似内容通过 [***订阅解码，我的 YouTube 频道***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) ***！***

# Vue 示例 1:使用 map()和 reduce()从购物车中获取总数

所以我们要解决的第一个例子可能是我们今天要解决的三个例子中最复杂的一个。所以让我们把困难的事情解决掉，因为它也可能是对你最有用的！

在我们的例子中，我们有一个电子商务网站。我们将把所有的条目存储在一个条目数组中，该数组位于我们的 data()函数*中(这是使用 Vue 组件时相当标准的做法)。*当我们将商品添加到购物车时，我们需要能够将整个商品对象添加到购物车中。因此，添加到购物车中的示例商品如下所示:

```
{
    id: 205,
    name: 'Banana',
    price: 1,
    imageSrc: Banana
}
```

现在考虑到这个例子主要是简单地获取总价，我们可以从这个 item 对象中添加价格。但实际上，这并没有多大用处，因为我们的购物车并不知道添加了什么商品。

因此，我们在这里希望发生的是，每次一个项目被添加到购物车，我们希望 Vue 以编程方式计算出总价格。这就是我们的计算属性发挥作用的地方！

让我们检查我们的示例代码，然后我们将解释发生了什么:

```
computed: {
    shoppingCartTotal() {
        **return this**.cart.map(item => item.price).reduce((total, amount) => total + amount);
    }
},
```

因此，我们首先在购物车数组上运行`map()`函数，该函数返回一个新数组，其中只包含每件商品的价格。然后我们立即调用`reduce()`函数，它将所有的值加在一起并返回总值。

但是 Vue 是如何知道每次在购物车中添加或删除商品时运行这个函数的呢？

嗯，在我们的例子中，每当一个商品被添加到购物车中(通过点击每个商品旁边写着“添加到购物车”的按钮)，该按钮都有一个附加的点击事件，它触发一个名为`updateCart()`的函数，如下所示:

```
methods: {
    updateCart(e) {
        **this**.cart.push(e);
        **this**.total = shoppingCartTotal;
    }
}
```

如您所见，它首先将一个商品推入我们的购物车。然后它调用`this.total`来分配`shoppingCartTotal`函数的值，该函数位于我们的计算属性部分中！多酷啊。！

# Vue 示例 2:将数据串连接在一起

现在这个例子和第三个例子会更传统一些，更符合你通常看到的计算属性的使用方式。让我们举一个例子，我们有一个充满用户的数据库，所有这些都被加载到我们的`data()`函数中。我们的数据结构有一个`name_first`和一个`name_last`值，我们希望找到一种有效的方法将这两个值连接在一起并显示在我们的页面上。为了更容易形象化，我们的用户对象应该是这样的:

```
user: {
    name_first: 'Sunil',
    name_last: 'Sandhu',
}
```

然后，我们可以使用模板/字符串文字在计算函数内将`name_first` 和`name_last`连接在一起，如下所示:

```
computed: {

    fullName() {
        **return** `${**this**.user.name_first} ${**this**.user.name_last}`
    }},
```

然后，在我们的视图中，我们可以引用`fullName`，就像我们在`data()`中引用任何其他值一样:

```
<p>{{ fullName }}</p>
```

哪会返回`Sunil Sandhu`！

# Vue 示例 3:一起计算数字

所以在我们的最后一个例子中，我们将使用 v-model 自动更新我们的`data()`对象中的值，然后使用一个计算属性将这两个数字相加。

首先，让我们看看我们的`data()`对象:

```
data() {
    **return** {
        num1: 0,
        num2: 0
    }
},
```

超级简单直接。为了避免`null`、`undefined` 和`NaN`出现任何问题，我们从一开始就将`0`附加到`num1`和`num2`。

我们的视图中有两个输入字段:

```
<input type="number" v-model="num1"/>
<input type="number" v-model="num2"/>
```

您可以看到，我们将 v-model 附加到两者上，值为`num1`和`num2`。因此，每当我们改变任一输入字段中的值时，这些数字也会在我们的`data()`对象中更新。

在我们的计算部分中，我们有这个函数:

```
computed: {
    answer() {
        **return this**.num1 + **this**.num2
    }
},
```

它只是将两个数相加。

最后，在我们看来，我们像这样输出我们的`answer()`函数的值:

```
<p>{{answer}}</p>
```

超级简单，我们可以通过添加一些按钮来触发加、减、乘、除，从而很容易地对其进行改进。

# 我们做到了！🎉

我们已经设法通过三个不同的，但现实生活中可用的例子来说明如何在 Vue 中使用计算属性来有效地更新数据。既然你已经知道如何使用计算属性，那就去构建许多很酷的东西吧！一定要分享，评论和喜欢这个帖子，让我知道你在 Vue 中做了什么了不起的事情！