# 单页应用如何打破网页设计

> 原文：<https://javascript.plainenglish.io/how-single-page-applications-broke-the-web-design-bd18d4ddcdab?source=collection_archive---------0----------------------->

![](img/fee382ae18120154c77e33932bc0f4c3.png)

*“Try Ember”* they said, “it will be fun” they said.

万维网最初是围绕通过超链接连接在一起的文档的概念而设计的。总的想法相当简单:一个 web 浏览器从服务器请求一个文档，下载它的内容并在屏幕上显示它(用 CSS 增强了视觉效果)。给定文档中的超链接导致另一个具有新内容的文档，从而形成了信息网络。网络浏览器、爬虫、社交媒体和整个网络基础设施都围绕这个简单的想法发展。

# 单页应用程序(spa)

单页应用程序是通过在 JavaScript 的帮助下重写网页内容来与用户交互的 web 应用程序。SPAs 不再为每次用户交互加载全新的文档，而是从外部 API 下载必要的数据，并通过减少加载时间来改善用户体验。这些应用程序对用户来说更自然，因为它们像普通的桌面应用程序而不是网站一样工作。

术语“单页应用程序”最早出现在 2002 年。三年后，AJAX 的引入为 Ember 或 Angular 等 SPA 框架提供了基础。每个框架旨在解决一个明确定义的问题。虽然 Angular 是针对已经熟悉 HTML 的 web 设计人员构建的，但 Ember 以其“约定优于配置”的原则减少了样板代码，而 2011 年推出的 ReactJS 凭借基于组件的方法、代码重用和速度起飞并颠覆了 web。

# 底层技术已经过时

React 等框架让开发人员能够轻松构建复杂的用户界面，否则使用传统方法几乎不可能创建这些界面。然而，它们运行的环境，即浏览器，在一定程度上是建立在静态网站的基础上的。

例如，URL 的概念对于基于文档的网站是有意义的，但是需要从 SPA 的角度重新考虑。URL 通常是通过使用浏览器的历史 API 人工注入的；这似乎是一种混合状态，而不是一个长期的解决方案。如果不小心处理，一些 spa 破坏了简单的功能，如浏览器中的前进和后退按钮，导致用户体验远非理想。即使是最简单的东西，比如 SPA 的 title 元素，也需要用 JavaScript 或服务器端呈现方法进行特殊处理。

另一个问题是网络爬虫。谷歌已经采取了一些措施来索引单页应用程序，但据谷歌员工约翰·穆勒(John Mueller)称，还有改进的空间:

> 它并不总是完美的，当然也不容易，但对于一些网站来说，它可以工作得很好，即使你依赖于客户端渲染(只有 JS，没有服务器端渲染)。YMMV :) — [约翰·穆勒](https://twitter.com/johnmu)

虽然脸书创建了 ReactJS，但令人惊讶的是，他们的爬虫对 JavaScript 的*let*和 *vars* 的混合完全视而不见，使得 spa 在他们的平台上经常不可见(鉴于该应用缺乏服务器端呈现的代码)。

# **国家管理很棘手**

采用单页面方法通常是有益的，但是必须记住保留应用程序状态带来的额外困难，这在传统的无状态方法中是不存在的。下面的场景虽然易于调试，但却说明了此问题的最简单形式:

*   一个用户(Richard)访问了您新开发的 React 单页应用程序，他对您在过去一个月左右的时间里一直在努力开发的极快的标签切换程序感到非常惊讶。
*   Richard 切换到注册选项卡，快速填写必要的字段，成为您的应用程序的荣誉成员。
*   作为会员，Richard 现在可以看到另外两个标签:**故事**和**阅读列表**。在所有的兴奋中，理查德打开了**故事**标签，并迅速在他的阅读列表中添加了两个故事。
*   然而，阅读列表仍然显示 **0 个故事**，这让理查德感到困惑，并使他删除了自己的帐户。

![](img/994c10582a2bfaef5b8fe6db4f99bd25.png)

在“过去”，实现的技术方面可能涉及两个不同的文档:stories.php 和 reading-list.php。这两个程序都有明确的生命周期——Richard 在浏览器中打开一个链接，程序运行几秒钟，然后返回一个 HTML 源代码，连脸书爬虫都能读懂。

在单页应用程序的情况下，通常有一个程序——app . js，在执行之前被传送到 Richard 的浏览器。然后，浏览器将逻辑操作翻译成 DOM 元素。

回到理查德，他遇到的问题是由于国家管理不当。程序员更新了按钮的状态(它们现在显示*添加到阅读列表中)，*但是忘记更新列表中的项目数。这个问题虽然很容易解决，但随着应用程序规模的增长，会变得更加棘手。Javascript 社区并不是唯一需要处理状态管理的社区，大多数移动开发人员也很熟悉这个问题。像 Redux 这样的工具在某种程度上有助于减少这种令人头痛的问题，但是会给项目带来样板文件和复杂性。

Web 开发人员正在从一个技术堆栈快速转移到另一个技术堆栈。单页应用程序之所以成为趋势，是因为与更传统的方法相比，它们在屏幕过渡方面提供了出色的用户体验。几乎每天都会发布新的框架和库，只是为了解决孤立的问题。向前迈出的真正一步是调整浏览器和万维网，使之更好地适应单页应用程序。在那之前，只会对下一个**“大事件”**进行炒作。