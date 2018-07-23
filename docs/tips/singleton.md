# Singleton Pattern

# 单例模式

The conventional singleton pattern is really something that is used to overcome the fact that all code must be in a `class`.

实际上，传统的单例模式是用来解决所有的代码都必须写在class中的事实。

```ts
class Singleton {
    private static instance: Singleton;
    private constructor() {
        // do something construct...
    }
    static getInstance() {
        if (!Singleton.instance) {
            Singleton.instance = new Singleton();
            // ... any one time initialization goes here ...
        }
        return Singleton.instance;
    }
    someMethod() { }
}

let something = new Singleton() // Error: constructor of 'Singleton' is private.

let instance = Singleton.getInstance() // do something with the instance...
```

However if you don't want lazy initialization you can instead just use a `namespace`: 

但是，如果你不想延迟初始化，你可以使用`namespace`：

```ts
namespace Singleton {
    // ... any one time initialization goes here ...
    export function someMethod() { }
}
// Usage
Singleton.someMethod();
```

> Warning : Singleton is just a fancy name for [global](http://stackoverflow.com/a/142450/390330)

> 警告：单例只是全局的一个花哨的名字(http://stackoverflow.com/a/142450/390330)

For most projects `namespace` can additionally be replaced by a *module*.

多余大多数项目，`namespace`还可以用*module*替换。

```ts
// someFile.ts
// ... any one time initialization goes here ...
export function someMethod() { }

// Usage
import {someMethod} from "./someFile";
```


