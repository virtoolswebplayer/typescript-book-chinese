# 你的 JavaScript 就是 TypeScript

曾经有（也将继续会有）很多竞争对手将*某种语法* 编译成 *JavaScript*。TypeScript 跟它们的区别在于*你的 JavaScript 就是 TypeScript*。这里是一张图：

![](https://raw.githubusercontent.com/basarat/typescript-book/master/images/venn.png)

然而这就意味着*你需要去学习 JavaScript*（好消息是*你**只**需要学习 JavaScript* ）。TypeScript 只是标准化了你在 JavaScript 上提供良好文档的方式。

* 只是给了你一个新的不会帮助修复 bug 的语法（就是在说你，CoffeeScript）。
* 创造了一个新的语言把你从你的运行时和社区中抽象得很远（就是在说你，Dart）。

TypeScript 仅仅是有文档的 JavaScript。

## 使 JavaScript 变得更好

TypeScript 会尝试把你从部分永远行不通的 JavaScript 中解救出来（所以不需要去记忆这些东西）：

```ts
[] + []; // JavaScript 会给你 ""（这没有意义），TypeScript 会报错

//
// 其他在 JavaScript 中没意义的东西 
// - 不会有运行时错误（让 debug 变得困难）
// - 但是 TypeScript 会给一个编译时错误（使 debug 不再必要）
//
{} + []; // JS : 0, TS Error
[] + {}; // JS : "[object Object]", TS Error  
{} + {}; // JS : NaN, TS Error
"hello" - 1; // JS : NaN, TS Error
```

实际上 TypeScript 是 JavaScript 的静态分析工具。只是比别的没有*类型信息*的静态分析工具做的更好。

## 你仍然需要学习 JavaScript

这就是说 TypeScript 非常忠于*你是在写 JavaScript*的事实，因此为了不会措手不及还有一些关于 JavaScript 的事情你需要去知道。接下来让我们去讨论它。

原文出处： https://github.com/ZenDay/TypeScipt-Deep-Dive-chinese-version/blob/master/docs/javascript/recap.md
