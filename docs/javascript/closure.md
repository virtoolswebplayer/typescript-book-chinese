## 闭包

JavaScript 的精华莫过于 `闭包`了。 闭包是 Javascript 中的一个函数，它可以访问外部作用域中定义的任意变量。到底什么是闭包，最好还是用例子来解释：

```ts
function outerFunction(arg) {
  var variableInOuterFunction = arg;

  function bar() {
    console.log(variableInOuterFunction); // 从外部范围访问变量
  }

  // 调用本地函数来证明它可以访问 arg
  bar();
}

outerFunction("hello closure"); // 打印 hello closure!
```

您可以看到内部函数可以从外部访问变量`variableInOuterFunction`。外部函数中的变量已被内部函数关闭（或约束）。因此,术语 **闭包** 这个概念本身很简单且非常直观。

**最妙的部分**：即使外部函数已执行完成并返回，内部函数仍然可以从外部访问变量。这是因为变量仍然绑定在内部函数中，而不依赖于外部函数。再来看一个例子：

```ts
function outerFunction(arg) {
  var variableInOuterFunction = arg;
  return function() {
    console.log(variableInOuterFunction);
  };
}

var innerFunction = outerFunction("hello closure!");

// 请注意outerFunction已返回
innerFunction(); // 打印 hello closure!
```

### 为什么说它绝妙

它使你更容易地组合对象，例如 [暴露模块模式](<(https://blog.csdn.net/cangdu/article/details/42497495)>)：

```ts
function createCounter() {
  let val = 0;
  return {
    increment() {
      val++;
    },
    getVal() {
      return val;
    }
  };
}

let counter = createCounter();
counter.increment();
console.log(counter.getVal()); // 1
counter.increment();
console.log(counter.getVal()); // 2
```

更高层次来说它也使实现像`nodejs`这样的东西成为可能（如果你现在还理解不了闭包的话也没有不关系，总有一天你会理解的 🌹）：

```ts
// 伪代码来解释这个概念
server.on(function handler(req, res) {
  loadData(req.id).then(function(data) {
    // `res` 已关闭并可用
    res.send(data);
  });
});
```
