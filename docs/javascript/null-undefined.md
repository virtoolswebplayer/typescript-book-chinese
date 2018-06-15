## Null and Undefined

## Null 和 Undefined

JavaScript (and by extension TypeScript) has two bottom types : `null` and `undefined`. They are _intended_ to mean different things:

JavaScript（和 TypeScript）有两种底层类型：`null` 和 `undefined`。他们意图表示不同的东西：

- Something hasn't been initialized : `undefined`.
- 有些东西还没有初始化：`undefined`。
- Something is currently unavailable: `null`.
- 有些东西目前不可用：`null`。

### Checking for either

### 检查两者之一

Fact is you will need to deal with both. Just check for either with `==` check.

事实是，如果你想判断某个变量是否是二者之一。使用 `==` 就可以。

```ts
/// 想象一下，你在做什么 `foo.bar == undefined` where bar 可以是其中之一:
console.log(undefined == undefined); // true
console.log(null == undefined); // true

// 您不必担心通过此检查造成的虚假价值
console.log(0 == undefined); // false
console.log("" == undefined); // false
console.log(false == undefined); // false
```

Recommend `== null` to check for both `undefined` or `null`. You generally don't want to make a distinction between the two.

推荐使用 `== null`来检查`undefined`或`null`。通常情况下我们无需区分他们。

```ts
function foo(arg: string | null | undefined) {
  if (arg != null) {
    // arg must be a string as `!=` rules out both null and undefined.
  }
}
```

One exception, root level undefined values which we discuss next.

一个例外，我们接下来讨论的根级未定义值 `undefined`。

### Checking for root level undefined

### 检查根级未定义

Remember how I said you should use `== null`. Of course you do (cause I just said it ^). Don't use it for root level things. In strict mode if you use `foo` and `foo` is undefined you get a `ReferenceError` **exception** and the whole call stack unwinds.

还记得我说你应该用'== null'。当然你确实记得（因为我刚刚说过）。**不要将它用于根级别的东西**。在严格模式下，如果您使用 `foo` 而 `foo` 未定义，则会出现 `ReferenceError` **异常**，整个调用堆栈将展开。

> You should use strict mode ... and in fact the TS compiler will insert it for you if you use modules ... more on those later in the book so you don't have to be explicit about it :)

> 你应该使用严格模式 ... 事实上，如果你使用模块，TS 编译器会为你插入......在本书后面的内容中更多，所以你不必明确说明它:)

So to check if a variable is defined or not at a _global_ level you normally use `typeof`:

因此，要检查一个变量是否定义在 global 级别，通常使用 `typeof`：

```ts
if (typeof someglobal !== "undefined") {
  // someglobal 可安全使用
  console.log(someglobal);
}
```

### Limit explicit use of `undefined`

### 限制显式使用`undefined`

Because TypeScript gives you the opportunity to _document_ your structures separately from values instead of stuff like:

由于 TypeScript 使您有机会独立于值而不是像下面这样的东西来记录结构：

```ts
function foo() {
  // if Something
  return { a: 1, b: 2 };
  // else
  return { a: 1, b: undefined };
}
```

you should use a type annotation:

你应该使用一个类型注释：

```ts
function foo(): { a: number; b?: number } {
  // if Something
  return { a: 1, b: 2 };
  // else
  return { a: 1 };
}
```

### Node style callbacks

### NodeJs 风格的回调函数

Node style callback functions (e.g. `(err,somethingElse)=>{ /* something */ }`) are generally called with `err` set to `null` if there isn't an error. You generally just use a truthy check for this anyways:

如果没有错误发生，NodeJs 风格的回调函数（例如`（err，somethingElse）=> {/ * something * /}`）的第一个参数 `err` 的值被设置为`null`。一般你只需检查 `err` 是否为真 ：

```ts
fs.readFile("someFile", "utf8", (err, data) => {
  if (err) {
    // do something
  } else {
    // no error
  }
});
```

When creating your own APIs it's _okay_ to use `null` in this case for consistency.
In all sincerity for your own APIs you should look at promises,in that case you actually don't need to bother with absent error values (you handle them with `.then` vs. `.catch`).

在创建自己的 API 时 _可以_ 使用 `null` 来保持一致性。
对于你自己的 API 来说，你应该看看 `promises`，
在这种情况下，你实际上不需要打扰缺少的错误值(你用`.then`和`.catch`处理它们).

### Don't use `undefined` as a means of denoting _validity_

### 不要使用 `undefined` 作为表示 _有效性的手段_

For example an awful function like this:

例如这样一个可怕的函数：

```ts
function toInt(str: string) {
  return str ? parseInt(str) : undefined;
}
```

can be much better written like this:

写成这样更好：

```ts
function toInt(str: string): { valid: boolean; int?: number } {
  const int = parseInt(str);
  if (isNaN(int)) {
    return { valid: false };
  } else {
    return { valid: true, int };
  }
}
```

### Final thoughts

### 最后的想法

TypeScript team doesn't use `null` : [TypeScript coding guidelines](https://github.com/Microsoft/TypeScript/wiki/Coding-guidelines#null-and-undefined) and it hasn't caused any problems. Douglas Crockford thinks [`null` is a bad idea](https://www.youtube.com/watch?v=PSGEjv3Tqo0&feature=youtu.be&t=9m21s) and we should all just use `undefined`.

TypeScript 团队不使用`null` : [TypeScript coding guidelines](https://github.com/Microsoft/TypeScript/wiki/Coding-guidelines#null-and-undefined) 并没有造成任何问题。道格拉斯克罗克福德认为 [`null` 是一个坏主意](https://www.youtube.com/watch?v=PSGEjv3Tqo0&feature=youtu.be&t=9m21s) 我们都应该使用 `undefined`.

However NodeJS style code bases uses `null` for Error arguments as standard as it denotes `Something is currently unavailable`. I personally don't care to distinguish between the two as most projects use libraries with differing opinions and just rule out both with `== null`.

但是，NodeJS 样式代码库使用`null`作为 Error 参数的标准，因为它表示`有些东西目前不可用`。我个人并不在意区分这两者，因为大多数项目都使用不同意见的库，只是用`== null`排除这两者。
