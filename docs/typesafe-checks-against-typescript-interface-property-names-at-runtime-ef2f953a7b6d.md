# Typesafe 运行时根据 TypeScript 接口属性名称进行检查

> 原文：<https://javascript.plainenglish.io/typesafe-checks-against-typescript-interface-property-names-at-runtime-ef2f953a7b6d?source=collection_archive---------4----------------------->

使用 TypeScript，接口只存在于开发期间。尽管如此，有时我们可能希望在运行时引用接口属性名。那我们能做什么？

![](img/aed64ce884b654d2b7fe08b628d15bdf.png)

在本文中，我将分享一个既酷又安全的技巧，以便能够在运行时引用接口属性名；也就是说，在 TS 编译器删除了我们接口的所有痕迹之后很久。

# TypeScript 接口消失

TypeScript 接口棒极了。厉害到运行时都不存在。

虽然这对于应用程序的包的重量来说很好(您可以在不影响包大小的情况下尽可能多地使用类型)，但有时这很烦人，因为我们仍然希望能够引用其中的一些类型。泛型也是如此，但那是另一个故事了！

例如，您可能希望对 JSON 数据进行运行时验证，以确保[输入确实符合您的期望](https://medium.com/@dSebastien/input-validation-with-nestjs-7184ba81af7e)。

在本文中，我不会深入探究接口“为什么”消失的细节，而是将重点放在如何执行在编译时验证的强类型运行时检查，以及如何防止重构。

# 一个具体的用例

在我的上一篇文章中，我基于我上周的工作讨论了[输入验证](https://medium.com/@dSebastien/input-validation-with-nestjs-7184ba81af7e)。在为我当前的项目集成输入验证时，我需要处理部分更新。

根据定义，部分更新(通常通过 HTTP POST 或 PATCH 请求发送)通常只包含应该在服务器端更新的数据的子集。部分更新请求可能包含一个或多个要更新的字段。当然，如果一个给定的字段存在，那么它需要被正确地验证。

通过[第一层输入验证](https://medium.com/@dSebastien/input-validation-with-nestjs-7184ba81af7e)后，我的 REST 控制器发送 [CQRS 命令](https://docs.nestjs.com/recipes/cqrs)，这些命令由命令处理程序负责。

该命令处理器接收部分更新数据，并且必须验证每个字段。在我的例子中，接收到的部分更新是基于如下所示的自定义类型:

因此，一个更新对象可能包含任何字段，这取决于`T`被设置为什么。在这种情况下，类型实际上是一个`PartialUpdate<MeetingDto>`。为了举例，我们假设`MeetingDto`只有以下属性:`A`、`B`和`C`。

在伪代码中，我的目标是做以下事情:

```
for each(property of partialUpdateObject) {
  // if property A of PartialUpdate<MeetingDto> is defined
    // then perform validations for A
  // ...
}
```

或者，我可以为每个属性写大量的 if 检查，但是我可能会漏掉一些；我更喜欢循环播放在场的人。

# 天真的实现

解决方案的最简单表达如下:

```
for (const propertyKey of ***Object***.keys(command.update)) {
  if("A" === propertyKey) {
    // perform validations for A
  }
}
```

但是如果你像我一样，那么你可能也不会喜欢这样做。你能看出问题吗？对于 JS 开发人员来说，这可能无关紧要，但是对于喜欢强类型的人来说，这完全是异端邪说。如果“A”属性曾经被重构/重命名；那么这段代码会立即中断，这就不好了。

那么我们能做些什么呢？

# 更清洁的解决方案

幸运的是，由于 [TypeScript 2.1](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-1.html) ，我们可以使用`keyof`关键字，也称为索引类型查询。使用`keyof`，我们可以获得类型`T`允许的属性名类型:

```
type PartialUpdateKeys = keyof PartialUpdate<MeetingDto>; // "A" | "B" | "C" | ...
```

我们快到了。现在的诀窍是将泛型与`keyof`结合起来，使属性名具有强类型，同时仍然能够在运行时与那些属性名进行比较。

我们可以通过以下自定义类型来实现这一点:

```
export const propertyOf = <SomeType>(name: keyof SomeType) => name;
```

在上面的表达式中，使用`propertyOf`时传递的泛型类型与`keyof`结合使用，以确保传入的值确实是该类型的有效属性名。最后，在输出中返回传入的名称。

通过使用这个，我们确实可以在运行时引用已知有效的属性名，即使接口已经不存在了。

这是如何使用的:

```
if(propertyOf<PartialUpdate<MeetingDto>>("A") === propertyKey) {
  // do something
}
```

这已经感觉更干净(也更安全！).

现在还有另一种变体可以使代码可读性稍微好一点:

```
export const propertiesOf = <SomeType>() => <T extends keyof SomeType>(name: T): T => name;
```

该变体可以如下使用:

```
const meetingUpdateProperties = propertiesOf<PartialUpdate<MeetingDto>();
...if(meetingUpdateProperties("A") === propertyKey) {
  // ...
}
```

我在 StackOverflow 上找到了这个技巧:[https://stack overflow . com/questions/33547583/safe-way-to-extract-property-names](https://stackoverflow.com/questions/33547583/safe-way-to-extract-property-names)

# 结论

在本文中，我解释了如何针对接口属性名实现安全的运行时属性名检查，即使那些接口在那个时候早已不存在了。

从类型安全的角度来看，这很适合我，使用起来也很安全；那么我们还能要求什么呢？

我很好奇你是否知道其他/更好/更安全的替代方案！

今天到此为止！

# 喜欢这篇文章吗？

如果你想了解关于软件/Web 开发、TypeScript、Angular、React、Vue、Kotlin、Java、Docker/Kubernetes 和其他很酷的主题的大量其他很酷的东西，那么不要犹豫[拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**