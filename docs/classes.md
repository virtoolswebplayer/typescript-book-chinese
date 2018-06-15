### Classes
### 类

The reason why it's important to have classes in JavaScript as a first class item is that:
为什么把class作为JavaScript的第一个项目来介绍，原因如下：
1. [Classes offer a useful structural abstraction](./tips/classesAreUseful.md)
1. Provides a consistent way for developers to use classes instead of every framework (emberjs,reactjs etc) coming up with their own version.
1. Object Oriented Developers already understand classes.

1. [Classes提供了非常有用的结构抽象](./tips/classesAreUseful.md)
1. 统一为开发人员提供一致的Classes规范，而不是每个框架（emberjs，reactjs等）提供自己的版本。
1. 熟悉面向对象的开发人员已经了解Class。

Finally JavaScript developers can *have `class`*. Here we have a basic class called Point:
最后，我们使用`class`，声明一个类Point：
```ts
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
    add(point: Point) {
        return new Point(this.x + point.x, this.y + point.y);
    }
}

var p1 = new Point(0, 10);
var p2 = new Point(10, 20);
var p3 = p1.add(p2); // {x:10,y:30}
```
This class generates the following JavaScript on ES5 emit:
Point这个calss在`ES5`中会生成如下代码去执行：
```ts
var Point = (function () {
    function Point(x, y) {
        this.x = x;
        this.y = y;
    }
    Point.prototype.add = function (point) {
        return new Point(this.x + point.x, this.y + point.y);
    };
    return Point;
})();
```
This is a fairly idiomatic traditional JavaScript class pattern now as a first class language construct.
这种标准的使用`class`的方式，已经是一流语言的标配了

### Inheritance
### 继承
Classes in TypeScript (like other languages) support *single* inheritance using the `extends` keyword as shown below:
在TypeScript中（和其他语言一样），使用关键字`extends`来实现 *single* 继承，示例：
```ts
class Point3D extends Point {
    z: number;
    constructor(x: number, y: number, z: number) {
        super(x, y);
        this.z = z;
    }
    add(point: Point3D) {
        var point2D = super.add(point);
        return new Point3D(point2D.x, point2D.y, this.z + point.z);
    }
}
```
If you have a constructor in your class then you *must* call the parent constructor from your constructor (TypeScript will point this out to you). This ensures that the stuff that it needs to set on `this` gets set. Followed by the call to `super` you can add any additional stuff you want to do in your constructor (here we add another member `z`).
如果在你声明的 `class`里面有一个`constructor`， 同时你还必须在你的`constructor`里面调用父类的`constructor`（TypeScript会提示你），这样能保证需要在`this`上设置的属性被正确的设置，接着再调用`super`，然后你就可以在你声明的`class`的`constructor`里面做你想做的事情了～

Note that you override parent member functions easily (here we override `add`) and still use the functionality of the super class in your members (using `super.` syntax).
注：这里你可以很容易的重写覆盖父类的方法（这里我们重写了`add`方法）并且仍然使用super类里面的成员方法（使用`super`方法）

### Statics
TypeScript classes support `static` properties that are shared by all instances of the class. A natural place to put (and access) them is on the class itself and that is what TypeScript does:
TypeScript类支持由该类的所有实例共享的“静态”属性。 一个放置（和访问）它们的自然地方是类本身，这就是TypeScript所做的事情：

```ts
class Something {
    static instances = 0;
    constructor() {
        Something.instances++;
    }
}

var s1 = new Something();
var s2 = new Something();
console.log(Something.instances); // 2
```

You can have static members as well as static functions.
你可以像写静态属性一样，写一个静态的方法

### Access Modifiers
TypeScript supports access modifiers `public`,`private` and `protected` which determine the accessibility of a `class` member as shown below:
TypeScript 支持访问修饰符 `public`,`private` 和 `protected`来修饰`class`成员的可访问性，如下所示:

| accessible on   | `public` | `protected` | `private` |
|-----------------|----------|-------------|-----------|
| class           | yes      | yes         | yes       |
| class children  | yes      | yes         | no        |
| class instances | yes      | no          | no        |


If an access modifier is not specified it is implicitly `public` as that matches the *convenient* nature of JavaScript 🌹.
如果未指定访问修饰符，则它隐式地为`public`，这也是JavaScript的特性，是不是很方便。

Note that at runtime (in the generated JS) these have no significance but will give you compile time errors if you use them incorrectly. An example of each is shown below:
注，在运行编译生成的js中，这些修饰符没有任何意义，但是如果您在`TypeScript`错误地使用修饰符，会在编译时候抛出错误。 示例：
```ts
class FooBase {
    public x: number;
    private y: number;
    protected z: number;
}

// EFFECT ON INSTANCES
var foo = new FooBase();
foo.x; // okay
foo.y; // ERROR : private
foo.z; // ERROR : protected

// EFFECT ON CHILD CLASSES
class FooChild extends FooBase {
    constructor() {
      super();
        this.x; // okay
        this.y; // ERROR: private
        this.z; // okay
    }
}
```

As always these modifiers work for both member properties and member functions.
这些修饰符适用于成员属性和成员函数。

### Abstract
`abstract` can be thought of as an access modifier. We present it separately because opposed to the previously mentioned modifiers it can be on a `class` as well as any member of the class. Having an `abstract` modifier primarily means that such functionality *cannot be directly invoked* and a child class must provide the functionality.
`abstract`可以被认为是一个访问修饰符。 我们单独提出它，因为与前面提到的修饰符相反，它可以在类中以及类中的任何成员上。 具有`abstract`修饰符主要意味着这样的功能*不能被直接调用*并且子类必须提供功能。

* `abstract` **classes** cannot be directly instantiated. Instead the user must create some `class` that inherits from the `abstract class`.
* 使用`abstract` **修饰的class** 不能被直接实例化. 相反的，我们必须声明一些 `class` 继承自 `abstract class`.
* `abstract` **members** cannot be directly accessed and a child class must provide the functionality.
* 使用`abstract` **修饰的成员方法** 不包含具体实现并且不能被直接访问，同时必须在派生类中实现。

### Constructor is optional（构造器是可选的）

The class does not need to have a constructor. e.g. the following is perfectly fine. 
`constructor`对于`class`不是必须的， 看代码：

```ts
class Foo {}
var foo = new Foo();
```

### Define using constructor

Having a member in a class and initializing it like below:
在`class`中初始化一个成员 看代码:

```ts
class Foo {
    x: number;
    constructor(x:number) {
        this.x = x;
    }
}
```
is such a common pattern that TypeScript provides a shorthand where you can prefix the member with an *access modifier* and it is automatically declared on the class and copied from the constructor. So the previous example can be re-written as (notice `public x:number`):
TypeScript提供了一种简写形式，您可以在该成员前用*访问修饰符*作为前缀，并在类中自动声明，并从构造函数中复制。这在TypeScript很常用。 所以前面的例子可以重写为（注意`public x：number`）：


```ts
class Foo {
    constructor(public x:number) {
    }
}
```

### Property initializer
This is a nifty feature supported by TypeScript (from ES7 actually). You can initialize any member of the class outside the class constructor, useful to provide default (notice `members = []`)
TypeScript支持属性初始化时直接赋值（自ES7）。 你可以在类的构造函数之外初始化类的任何成员，这对于提供默认值很有用（注意`members = []`）

```ts
class Foo {
    members = [];  // Initialize directly
    add(x) {
        this.members.push(x);
    }
}
```
