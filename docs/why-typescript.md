# Why TypeScript

# 为什么选择 TypeScript

There are two main goals of TypeScript:

使用 Typescript 主要有两个目的：

- Provide an _optional type system_ for JavaScript.
- Provide planned features from future JavaScript editions to current JavaScript engines

- TS 为`Javascript`提供了`可选类型系统`
- TS 可以让你使用`Javascript未来版本`计划支持的特性，并且运行在当前版本的 JavaScript 引擎上。

The desire for these goals is motivated below.

## The TypeScript type system

## Typescript 的类型系统

You might be wondering "**Why add types to JavaScript?**"

您可能想知道“**为什么要将类型添加到 JavaScript？**”

Types have proven ability to enhance code quality and understandability. Large teams (Google, Microsoft, Facebook) have continually arrived at this conclusion. Specifically:

类型已被证明能够提高代码质量和可读性。 大型团队（谷歌，微软，Facebook）一直在不断得出这个结论。 特别是：

- Types increase your agility when doing refactoring. _It's better for the compiler to catch errors than to have things fail at runtime_.
- 类型可以在重构时提高敏捷性。 _在编译期就捕捉错误总比在运行时失败要好得多_
- Types are one of the best forms of documentation you can have. _The function signature is a theorem and the function body is the proof_.
- 类型是您可以拥有的最佳文档形式之一。 _函数签名是一个定理，函数体是证明_。

However types have a way of being unnecessarily ceremonious. TypeScript is very particular about keeping the barrier to entry as low as possible. Here's how:

然而类型有着不必要的过分严谨。TypeScript 非常重视保持尽可能低的门槛。 就像接下来要说的几点：

### Your JavaScript is TypeScript

### 你的 JavaScript 就是 TypeScript

TypeScript provides compile time type safety for your JavaScript code. This is no surprise given its name. The great thing is that the types are completely optional. Your JavaScript code `.js` file can be renamed to a `.ts` file and TypeScript will still give you back valid `.js` equivalent to the original JavaScript file. TypeScript is _intentionally_ and strictly a superset of JavaScript with optional Type checking.

TypeScript 为您的 JavaScript 代码提供编译时类型安全性。 鉴于它的名字，这并不奇怪。 最棒的是这些类型是完全可选的。 您的 JavaScript 代码`.js`文件可以重命名为`.ts`文件，TypeScript 仍然会返回与原始 JavaScript 文件相同的有效`.js`。 TypeScript 有意而且严格地成为了有着可选类型检查的 JavaScript 的超集。

### Types can be Implicit

### 类型可以是隐式的

TypeScript will try to infer as much of the type information as it can in order to give you type safety with minimal cost of productivity during code development. For example, in the following example TypeScript will know that foo is of type `number` below and will give an error on the second line as shown:

TypeScript 将尝试尽可能多地推断出类型信息，以便在代码开发过程中以最低的生产力成本为您提供类型安全性。 例如，在以下示例中，TypeScript 会自动推断 foo 的类型为`number`，并在第二行显示错误，如下所示：

```ts
var foo = 123;
foo = "456"; // Error: cannot assign `string` to `number`

// Is foo a number or a string?
```

This type inference is well motivated. If you do stuff like shown in this example, then, in the rest of your code, you cannot be certain that `foo` is a `number` or a `string`. Such issues turn up often in large multi-file code bases. We will deep dive into the type inference rules later.

这种自动类型推断的行为效果很好。 如果你做了这个例子中所示的东西，那么在你的其他代码中，你不能确定`foo`究竟是一个`数字`或是`字符串`。 这些问题经常出现在大型多文件代码库中。 我们将在稍后深入探讨类型推断规则。

### Types can be Explicit

### 类型可以是显示的

As we've mentioned before, TypeScript will infer as much as it can safely, however you can use annotations to:

1.  Help along the compiler, and more importantly document stuff for the next developer who has to read your code (that might be future you!).
1.  Enforce that what the compiler sees, is what you thought it should see. That is your understanding of the code matches an algorithmic analysis of the code (done by the compiler).

就像我们之前提到过的一样，TypeScript 会尽可能安全地去推断，然而你也可以使用标注来：

1.  帮助编译器，更重要的是为需要阅读代码的下一位开发人员或你自己提供文档。
1.  强制编译器看到的就是您认为应该看到的内容。 这是您对代码的理解与代码的算法分析相匹配（由编译器完成）。

TypeScript uses postfix type annotations popular in other _optionally_ annotated languages (e.g. ActionScript and F#).

TypeScript 使用了在其他可选标注语言（例如 ActionScript 和 F#）中很流行的后缀类型标注。

```ts
var foo: number = 123;
```

So if you do something wrong the compiler will error e.g.:

因此如果做一些错误的事情，编译器就会报错 比如：

```ts
var foo: number = "123"; // Error: cannot assign a `string` to a `number`
```

We will discuss all the details of all the annotation syntax supported by TypeScript in a later chapter.

在下一章我们会讨论所有 TypeScript 支持的标注语法的细节。

### Types are structural

### 类型是结构化的

In some languages (specifically nominally typed ones) static typing results in unnecessary ceremony because even though _you know_ that the code will work fine the language semantics force you to copy stuff around. This is why stuff like [automapper for C#](http://automapper.org/) is _vital_ for C#. In TypeScript because we really want it to be easy for JavaScript developers with a minimum cognitive overload, types are _structural_. This means that _duck typing_ is a first class language construct. Consider the following example. The function `iTakePoint2D` will accept anything that contains all the things (`x` and `y`) it expects:

在某些语言（特别是那些名义上类型化的）中，静态类型导致了许多不必要的仪式，因为即使*你知道*代码可以正常运行，语言语义又迫使你不得不重复做一些事。 这就是为什么类似于[C＃的自动映射器]（http://automapper.org/）这样的东西对于C＃而言至关重要。 TypeScript 中因为我们真的想要以最小的认知超载来让 JavaScript 开发者更容易上手，所以类型是 _结构化的_。 这意味着 _鸭子类型_ 是第一类语言结构。 考虑下面的例子。 `iTakePoint2D`函数期望接受一个包含`x`和`y`属性的任意对象：

```ts
interface Point2D {
  x: number;
  y: number;
}
interface Point3D {
  x: number;
  y: number;
  z: number;
}
var point2D: Point2D = { x: 0, y: 10 };
var point3D: Point3D = { x: 0, y: 10, z: 20 };
function iTakePoint2D(point: Point2D) {
  /* do something */
}

iTakePoint2D(point2D); // exact match okay
iTakePoint2D(point3D); // extra information okay
iTakePoint2D({ x: 0 }); // Error: missing information `y`
```

### Type errors do not prevent JavaScript emit

### 类型错误不会阻止 JavaScript 生成

To make it easy for you to migrate your JavaScript code to TypeScript, even if there are compilation errors, by default TypeScript _will emit valid JavaScript_ the best that it can. e.g.

为了让你更容易地把 JavaScript 代码迁移到 TypeScript 上，尽管会有编译错误，默认情况下 TypeScript 还是会尽可能地生成有效的 JavaScript。例子：

```ts
var foo = 123;
foo = "456"; // Error: cannot assign a `string` to a `number`
```

will emit the following js:

会生成这样的 js：

```ts
var foo = 123;
foo = "456";
```

So you can incrementally upgrade your JavaScript code to TypeScript. This is very different from how many other language compilers work and yet another reason to move to TypeScript.

因此你可以增量更新你的 JavaScript 代码到 TypeScript。这跟其他语言编译器的工作机制有很大区别，也是另外一个使用 TypeScript 的原因。

### Types can be ambient

A major design goal of TypeScript was to make it possible for you to safely and easily use existing JavaScript libraries in TypeScript. TypeScript does this by means of _declaration_. TypeScript provides you with a sliding scale of how much or how little effort you want to put in your declarations, the more effort you put the more type safety + code intelligence you get. Note that definitions for most of the popular JavaScript libraries have already been written for you by the [DefinitelyTyped community](https://github.com/borisyankov/DefinitelyTyped) so for most purposes either:

TypeScript 的一个主要的设计目标就是让你能够安全而且轻松地在 TypeScript 中使用现存的 JavaScript 库。TypeScript 通过`定义`来做到这点。所以你在`声明`上投入的精力越多，你就能得到更多的类型安全和代码智能提醒。注意，绝大多数流行的 JavaScript 库的定义已经被 DefinitelyTyped 社区 写好了，所以对于绝大多数的目标而言：

1.  The definition file already exists.
1.  定义文件已经存在。
1.  Or at the very least, you have a vast list of well reviewed TypeScript declaration templates already available
1.  或者最起码，你有了一个大量的，被很好地审查过的已经可用的 TypeScript 定义模版的列表

As a quick example of how you would author your own declaration file, consider a trivial example of [jquery](https://jquery.com/). By default (as is to be expected of good JS code) TypeScript expects you to declare (i.e. use `var` somewhere) before you use a variable

这里用一个 jquery 的小例子来作为一个你如何编写你自己的定义文件的简单例子。默认情况下（也是好的 JS 代码所期望的），TypeScript 期望你在使用变量之前先（通过 var 或 let )定义变量

```ts
$(".awesome").show(); // Error: cannot find name `$`
```

As a quick fix _you can tell TypeScript_ that there is indeed something called `$`:

一个快速的修复方法是`你可以告诉 TypeScript` 的确有叫作 `$` 的东西：

```ts
declare var $: any;
$(".awesome").show(); // Okay!
```

If you want you can build on this basic definition and provide more information to help protect you from errors:

如果你希望你能够基于这个基本的定义来构建以及提供更多信息来帮助保护你避免出错的话：

```ts
declare var $: {
  (selector: string): any;
};
$(".awesome").show(); // Okay!
$(123).show(); // Error: selector needs to be a string
```

We will discuss the details of creating TypeScript definitions for existing JavaScript in detail later once you know more about TypeScript (e.g. stuff like `interface` and the `any`).

稍后我们将详细讨论为现有 JavaScript 创建 TypeScript `定义`的细节，你将会了解 TypeScript 更多的知识（例如“interface”和“any”之类的东西）。

## Future JavaScript => Now

## 未来的 JavaScript =>现在

TypeScript provides a number of features that are planned in ES6 for current JavaScript engines (that only support ES5 etc). The typescript team is actively adding these features and this list is only going to get bigger over time and we will cover this in its own section. But just as a specimen here is an example of a class:

TypeScript 为现在的 JavaScript 引擎（只支持 ES5 等等）提供了一系列在 ES6 中出现的特性。TypeScript 团队正积极地添加这些特性，这个列表只会随着时间的推移越来越大，而我们会在它们自己的章节讨论它。仅仅作为一个样品，这里是一个 class 的例子：

```ts
class Point {
  constructor(public x: number, public y: number) {}
  add(point: Point) {
    return new Point(this.x + point.x, this.y + point.y);
  }
}

var p1 = new Point(0, 10);
var p2 = new Point(10, 20);
var p3 = p1.add(p2); // { x: 10, y: 30 }
```

and the lovely fat arrow function:

以及可爱的箭头函数:

```ts
var inc = x => x + 1;
```

### Summary

### 结语

In this section we have provided you with the motivation and design goals of TypeScript. With this out of the way we can dig into the nitty gritty details of TypeScript.

在这节中我们为你提供了 TypeScript 的动机和设计目标。了解了这些之后，我们就可以深度挖掘 TypeScript 的细枝末节了。

[](Interfaces are open ended)
[](Type Inferernce rules)
[](Cover all the annotations)
[](Cover all ambients : also that there are no runtime enforcement)
[](.ts vs. .d.ts)
