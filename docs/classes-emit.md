#### What's up with the IIFE
#### IIFE（Immediately-Invoked Function Expression） 自执行函数做了些什么

The js generated for the class could have been:

在js中声明一个class，可以这样写：
```ts
function Point(x, y) {
    this.x = x;
    this.y = y;
}
Point.prototype.add = function (point) {
    return new Point(this.x + point.x, this.y + point.y);
};
```

The reason it's wrapped in an Immediately-Invoked Function Expression (IIFE) i.e.

为什么使用IIFE来包装呢？
```ts
(function () {

    // BODY

    return Point;
})();
```

has to do with inheritance. It allows TypeScript to capture the base class as a variable `_super` e.g.

原因与继承有关，TypeScript抽象出基础class，声明为`_super`变量，作为参数传入。

```ts
var Point3D = (function (_super) {
    __extends(Point3D, _super);
    function Point3D(x, y, z) {
        _super.call(this, x, y);
        this.z = z;
    }
    Point3D.prototype.add = function (point) {
        var point2D = _super.prototype.add.call(this, point);
        return new Point3D(point2D.x, point2D.y, this.z + point.z);
    };
    return Point3D;
})(Point);
```

Notice that the IIFE allows TypeScript to easily capture the base class `Point` in a `_super` variable and that is used consistently in the class body.

注意： 通过使用IIFE 使 TypeScript 很轻松的抽象出基础class `Point`赋值在`_super` 变量上，这样可以使在class内部统一的使用.

### `__extends`

You will notice that as soon as you inherit a class TypeScript also generates the following function:

你很快就会注意到，只要是继承了一个class，TypeScript就会生成下面的函数`__extends`：
```ts
var __extends = this.__extends || function (d, b) {
    for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];
    function __() { this.constructor = d; }
    __.prototype = b.prototype;
    d.prototype = new __();
};
```
Here `d` refers to the derived class and `b` refers to the base class. This function does two things:

`d` 指派生的class，`b`指基数class。这个函数做了两件事：

1. copies the static members of the base class onto the child class i.e. `for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];`
1. 复制基础class里面的静态成员到派生类中，即 `for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];`


1. sets up the child class function's prototype to optionally lookup members on the parent's `proto` i.e. effectively `d.prototype.__proto__ = b.prototype`
1. 设置子类的原型指向父类的原型 `d.prototype.__proto__ = b.prototype`

People rarely have trouble understanding 1, but many people struggle with 2. So an explanation is in order.
在理解上，很少有人对第一条有争议，但是很多人对于第二条存在争议

#### `d.prototype.__proto__ = b.prototype`

After having tutored many people about this I find the following explanation to be simplest. First we will explain how the code from `__extends` is equivalent to the simple `d.prototype.__proto__ = b.prototype`, and then why this line in itself is significant. To understand all this you need to know these things:

在教过很多人后，我发现下面的解释是最简单的。 首先，我们将解释`__extends`中的代码如何等价于简单的`d.prototype .__ proto__ = b.prototype`，以及为什么这行本身很重要。 要了解所有这些，你需要知道这些事情：

1. `__proto__`

1. `prototype`

1. effect of `new` on `this` inside the called function 
关键字`new`对于被调用函数内的`this`的影响
1. effect of `new` on `prototype` and `__proto__`
关键字`new`对于`prototype`和`__proto__`的影响


All objects in JavaScript contain a `__proto__` member. This member is often not accessible in older browsers (sometimes documentation refers to this magical property as `[[prototype]]`). It has one objective: If a property is not found on an object during lookup (e.g. `obj.property`) then it is looked up at `obj.__proto__.property`. If it is still not found then `obj.__proto__.__proto__.property` till either: *it is found* or *the latest `.__proto__` itself is null*. This explains why JavaScript is said to support *prototypal inheritance* out of the box. This is shown in the following example, which you can run in the chrome console or Node.js:

JavaScript中的所有对象都包含一个`__proto__`属性。 这个属性在旧版浏览器中通常无法访问（一些文档将这个属性称为`[[prototype]]`）。 `__proto__`有一个作用：在对象中查找某属性时没有在对象上找到属性（例如`obj.property`），那么它会在`obj .__ proto __。property`中继续查找。 如果它仍然没有找到，继续在`obj .__ proto __.__ proto __。property`查找，直到：*找到*或*不存在`.__ proto__`为止*。 这就是为什么JavaScript支持*原型继承*开箱即用（通过`__ proto __`实现js的原型链）。 以下示例显示了这一点，您可以在Chrome控制台或Node.js中运行该示例

```ts
var foo = {}

// setup on foo as well as foo.__proto__
foo.bar = 123;
foo.__proto__.bar = 456;

console.log(foo.bar); // 123
delete foo.bar; // remove from object
console.log(foo.bar); // 456
delete foo.__proto__.bar; // remove from foo.__proto__
console.log(foo.bar); // undefined
```

Cool so you understand `__proto__`. Another useful fact is that all `function`s in JavaScript have a property called `prototype` and that it has a member `constructor` pointing back to the function. This is shown below:

这就是你理解的 `__proto__`. js中所有的`function`s都有一个属性 `prototype`， 同时在这个属性中有一个成员 `constructor` 指回到这个function. 如下所示:

```ts
function Foo() { }
console.log(Foo.prototype); // {} i.e. it exists and is not undefined
console.log(Foo.prototype.constructor === Foo); // Has a member called `constructor` pointing back to the function
```

Now let's look at *关键字`new`对于被调用函数内的`this`的影响*. 正常情况下，在被调用函数内部 `this`指向通过该函数返回的新创建的对象. 这样就很容的可以判断出是否在函数内部改变了`this`指向:

现在让我们来看一下 *effect of `new` on `this` inside the called function*. Basically `this` inside the called function is going to point to the newly created object that will be returned from the function. It's simple to see if you mutate a property on `this` inside the function:

```ts
function Foo() {
    this.bar = 123;
}

// call with the new operator
var newFoo = new Foo();
console.log(newFoo.bar); // 123
```

Now the only other thing you need to know is that calling `new` on a function assigns the `prototype` of the function to the `__proto__` of the newly created object that is returned from the function call. Here is the code you can run to completely understand it:

现在还剩下唯一一个需要知道的是，通过 `new` 将函数的 `prototype` 赋值给该函数创建的新对象的 `__proto__`。 运行下面的代码帮助你更好的理解这句话:

```ts
function Foo() { }

var foo = new Foo();

console.log(foo.__proto__ === Foo.prototype); // True!
```

That's it. Now look at the following straight out of `__extends`. I've taken the liberty to number these lines:

就是这些. 看下面的 `__extends`. 我冒昧地对这些行编号：

```ts
1  function __() { this.constructor = d; }
2   __.prototype = b.prototype;
3   d.prototype = new __();
```

Reading this function in reverse the `d.prototype = new __()` on line 3 effectively means `d.prototype = {__proto__ : __.prototype}` (because of the effect of `new` on `prototype` and `__proto__`), combining it with the previous line (i.e. line 2 `__.prototype = b.prototype;`) you get `d.prototype = {__proto__ : b.prototype}`.

从后向前看，第三行的`d.prototype = new __()` 可以看成是 `d.prototype = {__proto__ : __.prototype}` (这就是上面提到的*关键字`new`对于`prototype`和`__proto__`的影响*), 与第2行一起看(`__.prototype = b.prototype;`) 就可以看成是 `d.prototype = {__proto__ : b.prototype}`。

But wait, we wanted `d.prototype.__proto__` i.e. just the proto changed and maintain the old `d.prototype.constructor`. This is where the significance of the first line (i.e. `function __() { this.constructor = d; }`) comes in. Here we will effectively have `d.prototype = {__proto__ : __.prototype, d.constructor = d}` (because of the effect of `new` on `this` inside the called function). So, since we restore `d.prototype.constructor`, the only thing we have truly mutated is the `__proto__` hence `d.prototype.__proto__ = b.prototype`.

停一下, 我们想要改变 `d.prototype.__proto__`，同时使用旧的的constructor `d.prototype.constructor`. 这就是第1行的作用 (`function __() { this.constructor = d; }`). 我们直接使用`d.prototype = {__proto__ : __.prototype, d.constructor = d}` (这就是上面提到的*关键字`new`对于被调用函数内的`this`的影响*). 因此, 我们恢复了constructor `d.prototype.constructor`, 函数执行完成后我们唯一改变的东西就是 `__proto__`， 让派生类的原型链指向了基础类的原型 `d.prototype.__proto__ = b.prototype`.

#### `d.prototype.__proto__ = b.prototype` 改变原型链指向的意义

The significance is that it allows you to add member functions to a child class and inherit others from the base class. This is demonstrated by the following simple example:

改变原型链的意义在于，派生类独有的方法属性可以在自身实现，并且可以从基础类继承公用的方法属性等，看下面的示例：

```ts
function Animal() { }
Animal.prototype.walk = function () { console.log('walk') };

function Bird() { }
Bird.prototype.__proto__ = Animal.prototype;
Bird.prototype.fly = function () { console.log('fly') };

var bird = new Bird();
bird.walk();
bird.fly();
```
`bird.fly` 鸟类的fly方法会在鸟类的原型链上继承得到 `bird.__proto__.fly` (记住关键字`new`使得`bird.__proto__`指向`Bird.prototype`) 同时 `bird.walk`鸟类的walk方法 (一个继承方法) 可以从基础类*Animal*`bird.__proto__.__proto__.walk`继承得到 (`bird.__proto__ == Bird.prototype`，`bird.__proto__.__proto__` == `Animal.prototype`)。
