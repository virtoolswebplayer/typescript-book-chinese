# Why TypeScript

# 为什么选择 TypeScript
There are two main goals of TypeScript:

使用Typescript主要有两个目的：

* Provide an *optional type system* for JavaScript.
* TS 为`Javascript`提供了`可选类型系统`
* Provide planned features from future JavaScript editions to current JavaScript engines
* TS 可以让你使用`Javascript未来版本`计划支持的特性，并且运行在当前版本的JavaScript引擎上。


The desire for these goals is motivated below.

## The TypeScript type system
## Typescript的类型系统

You might be wondering "**Why add types to JavaScript?**"

您可能想知道“**为什么要将类型添加到JavaScript？**”

Types have proven ability to enhance code quality and understandability. Large teams (Google, Microsoft, Facebook) have continually arrived at this conclusion. Specifically:

类型已被证明能够提高代码质量和可读性。 大型团队（谷歌，微软，Facebook）一直都得出这个结论。 特别：

* Types increase your agility when doing refactoring. *It's better for the compiler to catch errors than to have things fail at runtime*.
* 类型可以在重构时提高敏捷性。 *编译器捕捉错误比在运行时让事情失败更好。
* Types are one of the best forms of documentation you can have. *The function signature is a theorem and the function body is the proof*.
* 类型是您可以拥有的最佳文档形式之一。 *函数签名是一个定理，函数体是证明*。

However types have a way of being unnecessarily ceremonious. TypeScript is very particular about keeping the barrier to entry as low as possible. Here's how:

然而类型有一种不必要的仪式的方式。 TypeScript非常重视保持尽可能低的进入门槛。 就像这样：

### Your JavaScript is TypeScript

### 你的JavaScript就是TypeScript
TypeScript provides compile time type safety for your JavaScript code. This is no surprise given its name. The great thing is that the types are completely optional. Your JavaScript code `.js` file can be renamed to a `.ts` file and TypeScript will still give you back valid `.js` equivalent to the original JavaScript file. TypeScript is *intentionally* and strictly a superset of JavaScript with optional Type checking.

TypeScript为您的JavaScript代码提供编译时类型安全性。 鉴于它的名字，这并不奇怪。 最棒的是这些类型是完全可选的。 您的JavaScript代码`.js`文件可以重命名为`.ts`文件，TypeScript仍然会返回与原始JavaScript文件相同的有效`.js`。 TypeScript *是有意*的，并且严格地说是JavaScript的超集，可选的类型检查。

### Types can be Implicit

### 类型可以是隐式的

TypeScript will try to infer as much of the type information as it can in order to give you type safety with minimal cost of productivity during code development. For example, in the following example TypeScript will know that foo is of type `number` below and will give an error on the second line as shown:

TypeScript将尝试尽可能多地推断出类型信息，以便在代码开发过程中以最低的生产力成本为您提供类型安全性。 例如，在以下示例中，TypeScript将知道foo的类型为`number`，并在第二行显示错误，如下所示：

```ts
var foo = 123;
foo = '456'; // Error: cannot assign `string` to `number`

// Is foo a number or a string?
```
This type inference is well motivated. If you do stuff like shown in this example, then, in the rest of your code, you cannot be certain that `foo` is a `number` or a `string`. Such issues turn up often in large multi-file code bases. We will deep dive into the type inference rules later.

这种类型的推断很好动机。 如果你做了这个例子中所示的东西，那么在你的其他代码中，你不能确定`foo`是一个`数字'或'字符串`。 这些问题经常出现在大型多文件代码库中。 我们将在稍后深入探讨类型推断规则。

### Types can be Explicit

### 类型可以是显示的
As we've mentioned before, TypeScript will infer as much as it can safely, however you can use annotations to:

1. Help along the compiler, and more importantly document stuff for the next developer who has to read your code (that might be future you!).
1. Enforce that what the compiler sees, is what you thought it should see. That is your understanding of the code matches an algorithmic analysis of the code (done by the compiler).

正如我们之前提到的，TypeScript将尽可能地推断它，但是您可以使用注释来：
1. 帮助编译器，更重要的是为需要阅读代码的下一位开发人员提供文档（这可能是您的未来！）。
1. 强制编译器看到的是您认为应该看到的内容。 这是您对代码的理解与代码的算法分析相匹配（由编译器完成）。

TypeScript uses postfix type annotations popular in other *optionally* annotated languages (e.g. ActionScript and F#).

TypeScript使用Postfix类型注释，这些注释在其他*可选的注释语言（例如ActionScript和F＃）中非常流行。

```ts
var foo: number = 123;
```
So if you do something wrong the compiler will error e.g.:

```ts
var foo: number = '123'; // Error: cannot assign a `string` to a `number`
```

We will discuss all the details of all the annotation syntax supported by TypeScript in a later chapter.

### Types are structural

### 类型是结构化的
In some languages (specifically nominally typed ones) static typing results in unnecessary ceremony because even though *you know* that the code will work fine the language semantics force you to copy stuff around. This is why stuff like [automapper for C#](http://automapper.org/) is *vital* for C#. In TypeScript because we really want it to be easy for JavaScript developers with a minimum cognitive overload, types are *structural*. This means that *duck typing* is a first class language construct. Consider the following example. The function `iTakePoint2D` will accept anything that contains all the things (`x` and `y`) it expects:

在某些语言（特别是名义类型的语言）中，静态输入会导致不必要的仪式，因为即使*你知道*代码可以正常工作，语言语义迫使你复制东西。 这就是为什么类似于[C＃的automapper]（http://automapper.org/）这样的东西对于C＃而言至关重要。 在TypeScript中，因为我们确实希望它对于具有最小认知过载的JavaScript开发者来说很容易，所以类型是* structural *。 这意味着* duck typing *是第一类语言结构。 考虑下面的例子。 iTakePoint2D函数将接受任何包含所有期望的事物（`x`和`y`）：

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
var point2D: Point2D = { x: 0, y: 10 }
var point3D: Point3D = { x: 0, y: 10, z: 20 }
function iTakePoint2D(point: Point2D) { /* do something */ }

iTakePoint2D(point2D); // exact match okay
iTakePoint2D(point3D); // extra information okay
iTakePoint2D({ x: 0 }); // Error: missing information `y`
```

### Type errors do not prevent JavaScript emit
To make it easy for you to migrate your JavaScript code to TypeScript, even if there are compilation errors, by default TypeScript *will emit valid JavaScript* the best that it can. e.g.

```ts
var foo = 123;
foo = '456'; // Error: cannot assign a `string` to a `number`
```

will emit the following js:

```ts
var foo = 123;
foo = '456';
```

So you can incrementally upgrade your JavaScript code to TypeScript. This is very different from how many other language compilers work and yet another reason to move to TypeScript.

所以你可以逐步升级你的JavaScript代码到TypeScript。 这与其他语言编译器的工作方式有很大不同，也是转向TypeScript的另一个原因。

### Types can be ambient
A major design goal of TypeScript was to make it possible for you to safely and easily use existing JavaScript libraries in TypeScript. TypeScript does this by means of *declaration*. TypeScript provides you with a sliding scale of how much or how little effort you want to put in your declarations, the more effort you put the more type safety + code intelligence you get. Note that definitions for most of the popular JavaScript libraries have already been written for you by the [DefinitelyTyped community](https://github.com/borisyankov/DefinitelyTyped) so for most purposes either:

TypeScript的一个主要设计目标是使您可以安全，轻松地在TypeScript中使用现有的JavaScript库。 TypeScript通过*声明*来实现这一点。 TypeScript为您提供了一个可以在您的声明中投入多少或多少努力的滑动级别，您将越多的努力将更多类型的安全+代码智能付诸实践。 请注意，大多数流行的JavaScript库的定义已经由[DefinitelyTyped社区]（https://github.com/borisyankov/DefinitelyTyped）为您编写，因此大多数用途都是：

1. The definition file already exists.
2. Or at the very least, you have a vast list of well reviewed TypeScript declaration templates already available


1. 定义文件已经存在。
1. 至少，你有一个已经很好的评论TypeScript声明模板列表已经可用

As a quick example of how you would author your own declaration file, consider a trivial example of [jquery](https://jquery.com/). By default (as is to be expected of good JS code) TypeScript expects you to declare (i.e. use `var` somewhere) before you use a variable

作为你如何编写自己的声明文件的简单例子，考虑一个[jquery]（https://jquery.com/）的简单例子。 默认情况下（正如预期的好的JS代码一样）TypeScript希望在使用变量之前声明（即在某处使用`var`）

```ts
$('.awesome').show(); // Error: cannot find name `$`
```
As a quick fix *you can tell TypeScript* that there is indeed something called `$`:

作为一个快速修复*，您可以告诉TypeScript *确实有一些名为`$`的东西：

```ts
declare var $: any;
$('.awesome').show(); // Okay!
```
If you want you can build on this basic definition and provide more information to help protect you from errors:

如果你想要，你可以建立在这个基本定义上，并提供更多的信息来帮助你保护你免受错误：

```ts
declare var $: {
    (selector:string): any;
};
$('.awesome').show(); // Okay!
$(123).show(); // Error: selector needs to be a string
```

We will discuss the details of creating TypeScript definitions for existing JavaScript in detail later once you know more about TypeScript (e.g. stuff like `interface` and the `any`).

稍后我们将详细讨论为现有JavaScript创建TypeScript定义的细节，稍后会详细了解TypeScript（例如“interface”和“any”之类的东西）。

## Future JavaScript => Now

## 未来的JavaScript =>现在

TypeScript provides a number of features that are planned in ES6 for current JavaScript engines (that only support ES5 etc). The typescript team is actively adding these features and this list is only going to get bigger over time and we will cover this in its own section. But just as a specimen here is an example of a class:

TypeScript提供了许多在ES6中为当前JavaScript引擎（仅支持ES5等）计划的功能。 打字团队正在积极添加这些功能，而且这个列表随着时间的推移只会变得更大，我们将在其自己的章节中介绍。 但就像这里的一个样本是一个类的例子：

```ts
class Point {
    constructor(public x: number, public y: number) {
    }
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
var inc = x => x+1;
```

### Summary

### 总结

In this section we have provided you with the motivation and design goals of TypeScript. With this out of the way we can dig into the nitty gritty details of TypeScript.

在本节中，我们向您提供了TypeScript的动机和设计目标。 通过这种方式，我们可以深入了解TypeScript的基本细节。

[](Interfaces are open ended)
[](Type Inferernce rules)
[](Cover all the annotations)
[](Cover all ambients : also that there are no runtime enforcement)
[](.ts vs. .d.ts)
