* [Arrow Functions](#arrow-functions)
* [Tip: Arrow Function Need](#tip-arrow-function-need)
* [Tip: Arrow Function Danger](#tip-arrow-function-danger)
* [Tip: Libraries that use `this`](#tip-arrow-functions-with-libraries-that-use-this)
* [Tip: Arrow Function inheritance](#tip-arrow-functions-and-inheritance)
* [Tip: Quick object return](#tip-quick-object-return)

* [箭头函数](#arrow-functions)
* [Tip: Arrow Function Need](#tip-arrow-function-need)
* [Tip: Arrow Function Danger](#tip-arrow-function-danger)
* [Tip: Libraries that use `this`](#tip-arrow-functions-with-libraries-that-use-this)
* [Tip: Arrow Function inheritance](#tip-arrow-functions-and-inheritance)
* [Tip: Quick object return](#tip-quick-object-return)

### Arrow Functions
### 箭头函数

Lovingly called the *fat arrow* (because `->` is a thin arrow and `=>` is a fat arrow) and also called a *lambda function* (because of other languages). Another commonly used feature is the fat arrow function `()=>something`. The motivation for a *fat arrow* is:
1. You don't need to keep typing `function`
2. It lexically captures the meaning of `this`
2. It lexically captures the meaning of `arguments`

形象的称为*胖箭头*（因为` ->`是一个瘦箭头，`=>`是一个胖箭头），也称为* lambda函数*（因为借鉴自其他语言）。 另一个常用的功能是胖箭头函数`（）=> something`。 *胖箭*的动机是：
1. 不需要使用`function`关键字了
2. 不需要关心`this`的指向问题了
3. `arguments` 在箭头函数里面是不可用的，注意哦

For a language that claims to be functional, in JavaScript you tend to be typing `function` quite a lot. The fat arrow makes it simple for you to create a function

对于需要函数功能的语言， 就像JavaScript，你需要写很多次声明关键字 `function`， 箭头函数能帮助你化简了很多无聊的工作，很容易的声明一个函数

```ts
var inc = (x)=>x+1;
```
`this` has traditionally been a pain point in JavaScript. As a wise man once said "I hate JavaScript as it tends to lose the meaning of `this` all too easily". Fat arrows fix it by capturing the meaning of `this` from the surrounding context. Consider this pure JavaScript class:

`this`一直是JavaScript的痛点，作为一个聪明人曾经说过“我讨厌JavaScript，因为它很容易失去`this`的含义”，箭头函数解决了这个痛点，它会根据上下文信息绑定正确的`this`, 思考下下面的这个class：
```ts
function Person(age) {
    this.age = age;
    this.growOld = function() {
        this.age++;
    }
}
var person = new Person(1);
setTimeout(person.growOld,1000);

setTimeout(function() { console.log(person.age); },2000); // 1, should have been 2
```
If you run this code in the browser `this` within the function is going to point to `window` because `window` is going to be what executes the `growOld` function. 
Fix is to use an arrow function:

如果你在浏览器中运行这个代码，那么函数中的`this`将指向`window`，因为`window`将执行`growOld`函数。
通过箭头函数解决这个问题：
```ts
function Person(age) {
    this.age = age;
    this.growOld = () => {
        this.age++;
    }
}
var person = new Person(1);
setTimeout(person.growOld,1000);

setTimeout(function() { console.log(person.age); },2000); // 2
```
The reason why this works is the reference to `this` is captured by the arrow function from outside the function body. This is equivalent to the following JavaScript code (which is what you would write yourself if you didn't have TypeScript):

为什么能解决这个问题，是因为对`this`的引用是由函数体外部的箭头函数捕获的，这相当于以下JavaScript代码（如果您没有TypeScript，这是您自己编写的代码）：
```ts
function Person(age) {
    this.age = age;
    var _this = this;  // capture this
    this.growOld = function() {
        _this.age++;   // use the captured this
    }
}
var person = new Person(1);
setTimeout(person.growOld,1000);

setTimeout(function() { console.log(person.age); },2000); // 2
```
Note that since you are using TypeScript you can be even sweeter in syntax and combine arrows with classes:
注意： 这里使用的是ts，才把箭头函数和class组合在一起的，如果是js，稳妥起见还是用老办法吧，或者你可以通过babel来实现
```ts
class Person {
    constructor(public age:number) {}
    growOld = () => {
        this.age++;
    }
}
var person = new Person(1);
setTimeout(person.growOld,1000);

setTimeout(function() { console.log(person.age); },2000); // 2
```

> [A sweet video about this pattern 🌹](https://egghead.io/lessons/typescript-make-usages-of-this-safe-in-class-methods)
> [非常好的视频 🌹](https://egghead.io/lessons/typescript-make-usages-of-this-safe-in-class-methods)

#### Tip: Arrow Function Need
Beyond the terse syntax, you only *need* to use the fat arrow if you are going to give the function to someone else to call. Effectively:

除了简洁的语法之外，如果需要他人调用这个功能， 看效果：
```ts
var growOld = person.growOld;
// Then later someone else calls it:
growOld();
```
If you are going to call it yourself, i.e.
```ts
person.growOld();
```
then `this` is going to be the correct calling context (in this example `person`).
在这里 `this` 还是原来的`this` (这个例子里面的`person`)

#### Tip: Arrow Function Danger

In fact if you want `this` *to be the calling context* you should *not use the arrow function*. This is the case with callbacks used by libraries like jquery, underscore, mocha and others. If the documentation mentions functions on `this` then you should probably just use a `function` instead of a fat arrow. Similarly if you plan to use `arguments` don't use an arrow function.

#### Tip: Arrow functions with libraries that use `this`
Many libraries do this e.g. `jQuery` iterables (one example https://api.jquery.com/jquery.each/) will use `this` to pass you the object that it is currently iterating over. In this case if you want to access the library passed `this` as well as the surrounding context just use a temp variable like `_self` like you would in the absence of arrow functions.

事实上，如果你想要`this` *作为调用上下文*你应该*不使用箭头函数*。jquery(一个例子：https://api.jquery.com/jquery.each/)。 不想Object来影响原有`this`的指向，在这个例子中，如果你不想`this`的指向发生改变，可以使用一个临时变量`_self`， 就像之前的例子一样。

```ts
let _self = this;
something.each(function() {
    console.log(_self); // the lexically scoped value
    console.log(this); // the library passed value
});
```

#### Tip: Arrow functions and inheritance
Arrow functions as properties on classes work fine with inheritance: 

箭头函数作为类的属相，继承没什么问题
```ts
class Adder {
    constructor(public a: number) {}
    add = (b: number): number => {
        return this.a + b;
    }
}
class Child extends Adder {
    callAdd(b: number) {
        return this.add(b);
    }
}
// Demo to show it works
const child = new Child(123);
console.log(child.callAdd(123)); // 246
```

However they do not work with the `super` keyword when you try to override the function in a child class. Properties go on `this`. Since there is only one `this` such functions cannot participate in a call to `super` (`super` only works on prototype members). You can easily get around it by creating a copy of the method before overriding it in the child.

然而他们不能通过`super`关键字来正常工作。 由于只有一个`this`这样的函数不能参与调用`super`（`super`只适用于原型成员）。 您可以通过在覆盖它之前创建方法的副本来轻松绕过它。

```ts
class Adder {
    constructor(public a: number) {}
    // This function is now safe to pass around
    add = (b: number): number => {
        return this.a + b;
    }
}

class ExtendedAdder extends Adder {
    // Create a copy of parent before creating our own
    private superAdd = this.add;
    // Now create our override
    add = (b: number): number => {
        return this.superAdd(b);
    }
}
```

### Tip: Quick object return

Sometimes you need a function that just returns a simple object literal. However, something like

有些时候你需要函数返回一个简单的object， 例子：
```ts
// WRONG WAY TO DO IT
var foo = () => {
    bar: 123
};
```
is parsed as a *block* containing a *JavaScript Label* by JavaScript runtimes (cause of the JavaScript specification).
被程序认为是函数体的标志了， 并没有返回object

>  If that doesn't make sense, don't worry, as you get a nice compiler error from TypeScript saying "unused label" anyways. Labels are an old (and mostly unused) JavaScript feature that you can ignore as a modern *GOTO considered bad* experienced developer 🌹

如果这没有意义，不要担心，因为TypeScript会给你一个编译错误`unused label`。 标签是一种旧的（几乎没人使用的）JavaScript功能，作为现代经验丰富的开发人员你完全可以忽略他

You can fix it by surrounding the object literal with `()`:
你可以修复这个问题，只是简单的用`()`包住object

```ts
// Correct 🌹
var foo = () => ({
    bar: 123
});
```
