《typescript deep drive》是一本 typescript 实战书，本书讲解了 typescript 的一些核心知识和常见问题，目前还没有中文版本。so 突发奇想，由社区驱动社区共建项目的方式，把这本书译成中文，以方便阅读推广。请大家跟贴报名，自领章节。

## 如何参与翻译

首先你要有一个 github 帐号 如果没有请先 [注册 GitHub 帐号](https://github.com/join?source=header-home)

克隆项目到本地

```sh
git clone --depth 1 git@github.com:virtoolswebplayer/typescript-book-chinese.git
```

翻译-->保存-->提交

> 为了避免翻译冲突，请大家在翻译大前  务必先在更新 `翻译计划` 中对应的文章标题上加入自己的名字

章节领取规则为： `文章标题 [译：姓名]` ，姓名可为中文名或 github 帐户名，一定要让我知道你是谁^\_^

例如：[Getting Started](docs/getting-started.md) [译：高乐天]

## 翻译计划

- [Getting Started](docs/getting-started.md) [译：高乐天]
  - [Why TypeScript](docs/why-typescript.md) [译：高乐天]
- [JavaScript](docs/javascript/recap.md) [译：高乐天]
  - [Equality](docs/javascript/equality.md) [译：高乐天]
  - [References](docs/javascript/references.md) [译：高乐天]
  - [Null vs. Undefined](docs/javascript/null-undefined.md) [译：高乐天]
  - [this](docs/javascript/this.md) [译：高乐天]
  - [Closure](docs/javascript/closure.md) [译：高乐天]
  - [Number](docs/javascript/number.md) [译：高乐天]
- [Future JavaScript Now](docs/future-javascript.md)
  - [Classes](docs/classes.md)
    - [Classes Emit](docs/classes-emit.md)
  - [Arrow Functions](docs/arrow-functions.md)
  - [Rest Parameters](docs/rest-parameters.md)
  - [let](docs/let.md)
  - [const](docs/const.md)
  - [Destructuring](docs/destructuring.md)
  - [Spread Operator](docs/spread-operator.md)
  - [for...of](docs/for...of.md)
  - [Iterators](docs/iterators.md)
  - [Template Strings](docs/template-strings.md)
  - [Promise](docs/promise.md)
  - [Generators](docs/generators.md)
  - [Async Await](docs/async-await.md)
- [Project](docs/project/project.md)
  - [Compilation Context](docs/project/compilation-context.md)
    - [tsconfig.json](docs/project/tsconfig.md)
    - [Which Files?](docs/project/files.md)
  - [Declaration Spaces](docs/project/declarationspaces.md)
  - [Modules](docs/project/modules.md)
    - [File Module Details](docs/project/external-modules.md)
    - [globals.d.ts](docs/project/globals.md)
  - [Namespaces](docs/project/namespaces.md)
  - [Dynamic Import Expressions](docs/project/dynamic-import-expressions.md)
- [Node.js QuickStart](docs/quick/nodejs.md)
- [Browser QuickStart](docs/quick/browser.md)
- [TypeScript's Type System](docs/types/type-system.md)
  - [JS Migration Guide](docs/types/migrating.md)
  - [@types](docs/types/@types.md)
  - [Ambient Declarations](docs/types/ambient/intro.md)
    - [Declaration Files](docs/types/ambient/d.ts.md)
    - [Variables](docs/types/ambient/variables.md)
  - [Interfaces](docs/types/interfaces.md)
  - [Enums](docs/enums.md)
  - [`lib.d.ts`](docs/types/lib.d.ts.md)
  - [Functions](docs/types/functions.md)
  - [Callable](docs/types/callable.md)
  - [Type Assertion](docs/types/type-assertion.md)
  - [Freshness](docs/types/freshness.md)
  - [Type Guard](docs/types/typeGuard.md)
  - [Literal Types](docs/types/literal-types.md)
  - [Readonly](docs/types/readonly.md)
  - [Generics](docs/types/generics.md)
  - [Type Inference](docs/types/type-inference.md)
  - [Type Compatibility](docs/types/type-compatibility.md)
  - [Never Type](docs/types/never.md)
  - [Discriminated Unions](docs/types/discriminated-unions.md)
  - [Index Signatures](docs/types/index-signatures.md)
  - [Moving Types](docs/types/moving-types.md)
  - [Exception Handling](docs/types/exceptions.md)
  - [Mixins](docs/types/mixins.md)
- [JSX](docs/jsx/tsx.md)
  - [React](docs/jsx/react.md)
  - [Non React JSX](docs/jsx/others.md)
- [Options](docs/options/intro.md)
  - [noImplicitAny](docs/options/noImplicitAny.md)
  - [strictNullChecks](docs/options/strictNullChecks.md)
- [Testing](docs/testing/intro.md)
  - [Jest](docs/testing/jest.md)
- [Tools](docs/tools/intro.md)
  - [Prettier](docs/tools/prettier.md)
  - [Husky](docs/tools/husky.md)
  - [Changelog](docs/tools/changelog.md)
- [TIPs](docs/tips/main.md)
  - [String Based Enums](docs/tips/stringEnums.md)
  - [Nominal Typing](docs/tips/nominalTyping.md)
  - [Stateful Functions](docs/tips/statefulFunctions.md)
  - [Bind is Bad](docs/tips/bind.md)
  - [Currying](docs/tips/currying.md)
  - [Type Instantiation](docs/tips/typeInstantiation.md)
  - [Lazy Object Literal Initialization](docs/tips/lazyObjectLiteralInitialization.md)
  - [Classes are Useful](docs/tips/classesAreUseful.md)
  - [Avoid Export Default](docs/tips/defaultIsBad.md)
  - [Limit Property Setters](docs/tips/propertySetters.md)
  - [`outFile` caution](docs/tips/outFile.md)
  - [JQuery tips](docs/tips/jquery.md)
  - [static constructors](docs/tips/staticConstructor.md)
  - [singleton pattern](docs/tips/singleton.md)
  - [Function parameters](docs/tips/functionParameters.md)
  - [Truthy](docs/tips/truthy.md)
  - [Build Toggles](docs/tips/build-toggles.md)
  - [Barrel](docs/tips/barrel.md)
  - [Create Arrays](docs/tips/create-arrays.md)
  - [Typesafe Event Emitter](docs/tips/typed-event.md)
- [StyleGuide](docs/styleguide/styleguide.md)
- [Common Errors](docs/errors/main.md)
- [TypeScript Compiler Internals](docs/compiler/overview.md)
  - [Program](docs/compiler/program.md)
  - [AST](docs/compiler/ast.md)
    - [TIP: Visit Children](docs/compiler/ast-tip-children.md)
    - [TIP: SyntaxKind enum](docs/compiler/ast-tip-syntaxkind.md)
    - [Trivia](docs/compiler/ast-trivia.md)
  - [Scanner](docs/compiler/scanner.md)
  - [Parser](docs/compiler/parser.md)
    - [Parser Functions](docs/compiler/parser-functions.md)
  - [Binder](docs/compiler/binder.md)
    - [Binder Functions](docs/compiler/binder-functions.md)
    - [Binder Declarations](docs/compiler/binder-declarations.md)
    - [Binder Container](docs/compiler/binder-container.md)
    - [Binder SymbolTable](docs/compiler/binder-symboltable.md)
    - [Binder Error Reporting](docs/compiler/binder-diagnostics.md)
  - [Checker](docs/compiler/checker.md)
    - [Checker Diagnostics](docs/compiler/checker-global.md)
    - [Checker Error Reporting](docs/compiler/checker-diagnostics.md)
  - [Emitter](docs/compiler/emitter.md)
    - [Emitter Functions](docs/compiler/emitter-functions.md)
    - [Emitter SourceMaps](docs/compiler/emitter-sourcemaps.md)
  - [Contributing](docs/compiler/contributing.md)
