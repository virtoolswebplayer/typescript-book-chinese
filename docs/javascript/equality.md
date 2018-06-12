## Equality

## 相等

One thing to note in JavaScript is the difference between `==` and `===`. As JavaScript tries to 
be resilient against programming errors `==` tries to do type coercion between two variables e.g. converts 
a string to a number so that you can compare with a number as shown below:

在JavaScript中需要注意的一点是`==`和`===`之间的区别。由于JavaScript试图对编程错误具有弹性，
所以`==`试图在两个变量之间进行类型强制转换，例如将字符串转换为数字，以便可以与数字进行比较，如下所示：

```js
console.log(5 == "5"); // true   , TS Error
console.log(5 === "5"); // false , TS Error
```

However the choices JavaScript makes are not always ideal. For example in the below example the first statement is false
because `""` and `"0"` are both strings and are clearly not equal. However in the second case both `0` and the
empty string (`""`) are falsy (i.e. behave like `false`) and are therefore equal with respect to `==`. Both statements
are false when you use `===`.

然而，JavaScript所做的选择并不总是理想的。例如在下面的例子中，第一个语句是错误的
因为`""`和`"0"`都是字符串，显然不相等。然而在第二种情况下，`0`和空字符串（`“”`）是虚假的（即像“假”一样），
因此使用`==`比较结果相等的，而在使用`===`比较时则是错误的。

```js
console.log("" == "0"); // false
console.log(0 == ""); // true

console.log("" === "0"); // false
console.log(0 === ""); // false
```

> Note that `string == number` and `string === number` are both compile time errors in TypeScript, so you don't normally need to worry about this.

> 请注意`string == number`和 `string === number` 在TypeScript中都是编译时错误，所以你通常不需要担心这一点。

Similar to `==` vs. `===`, there is `!=` vs. `!==`

`==`和`===`类似，有`!=`和`!==`

So ProTip: Always use `===` and `!==` except for null checks, which we cover later.

所以ProTip：除了空检查之外，总是使用`===`和`!==`，我们稍后会介绍。

## Structural Equality 

## 结构平等

If you want to compare two objects for structural equality `==`/`===` are ***not*** sufficient. e.g. 

如果你想比较两个结构相等的对象`==`/`===`是不够的。例如

```js
console.log({a:123} == {a:123}); // False
console.log({a:123} === {a:123}); // False
```
To do such checks use the [deep-equal](https://www.npmjs.com/package/deep-equal) npm package e.g. 

要做这样的检查，使用 [deep-equal](https://www.npmjs.com/package/deep-equal) npm软件包，例如:

```js
import * as deepEqual from "deep-equal";

console.log(deepEqual({a:123},{a:123})); // True
```
