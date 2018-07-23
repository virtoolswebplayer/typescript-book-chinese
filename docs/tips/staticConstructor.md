# Static Constructors in TypeScript

# TypeScript中的静态构造函数

TypeScript `class` (like JavaScript `class`) cannot have a static constructor. However you can get the same effect quite easily by just calling it yourself:

和JavaScript中的class一样，TypeScript中的class不能有静态构造函数。但是，你可以通过自己调用来达到同样目的。

```ts
class MyClass {
    static initialize() {
        // Initialization
    }
}
MyClass.initialize();
```
