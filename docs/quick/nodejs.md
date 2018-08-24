# TypeScript with Node.js

# TypeScript 与 Node.js

TypeScript has had *first class* support for Node.js since inception. Here's how to setup a quick Node.js project:

TypeScript 从最初就对 Node.js 有着*头等*支持。下面是如何快速开始一个 NodeJS 项目：

> Note: many of these steps are actually just common practice Node.js setup steps

> 注意：这里的很多步骤实际上只是普通的 Node.js 配置步骤

1. Setup a Node.js project `package.json`. Quick one : `npm init -y`

1. 配置一个 Node.js 项目的 `package.json`。快捷方法：`npm init -y`

1. Add TypeScript (`npm install typescript --save-dev`)

1. 添加 TypeScript（`npm install typescript --save-dev`）

1. Add `node.d.ts` (`npm install @types/node --save-dev`)

1. 添加 `node.d.ts`（`npm install @types/node --save-dev`）

1. Init a `tsconfig.json` for TypeScript options (`npx tsc --init`)

1. 为 TypeScript 设置初始化一个 `tsconfig.json`（`node ./node_modules/.bin/tsc --init`）

1. Make sure you have `compilerOptions.module:commonjs` in your tsconfig.json

1. 确保你的 tsconfig.json 里有 `compilerOptions.module:commonjs`

That's it! Fire up your IDE (e.g. `alm -o`) and play around. Now you can use all the built in node modules (e.g. `import fs = require('fs');`) with all the safety and developer ergonomics of TypeScript!

就是这样！启动你的 IDE（例如 `alm -o`）然后开始尝试。现在你可以使用所有内置 node 模块（例如 `import fs = require('fs')`）并享受 TypeScript 的安全与舒适。

## Bonus: Live compile + run

## 另外: 实时编译 + 运行

* Add `ts-node` which we will use for live compile + run in node (`npm install ts-node --save-dev`)

* 添加 `ts-node` 以便在 node 下实时编译 + 运行（`npm install ts-node --save-dev`）

* Add `nodemon` which will invoke `ts-node` whenever a file is changed (`npm install nodemon --save-dev`)

* 添加 `nodemon` 以便在任何文件发生改变时调用 `ts-node`（`npm install nodemon --save-dev`）

Now just add a `script` target to your `package.json` based on your application entry e.g. assuming its `index.ts`:

现在只需要在 `package.json` 里根据你的应用的入口添加 `script` 条目, 比如 `index.ts`:

```json
  "scripts": {
    "start": "npm run build:live",
    "build:live": "nodemon --exec ./node_modules/.bin/ts-node -- ./index.ts"
  },
```

So you can now run `npm start` and as you edit `index.ts`:

现在你可以运行 `npm start` , 在你编辑 `index.ts` 时:

* nodemon reruns its command (ts-node)

* nodemon 会重新运行它的命令（ts-node）

* ts-node transpiles automatically picking up tsconfig.json and the installed typescript version,

* ts-node 根据 tsconfig.json 和安装的 typescript 版本进行自动转换

* ts-node runs the output javascript through Node.js.

* ts-node 使用 Node.js 运行输出的 javascript

## Creating TypeScript node modules

## 创建 TypeScript node 模块

* [A lesson on creating typescript node modules](https://egghead.io/lessons/typescript-create-high-quality-npm-packages-using-typescript)

* [一个创建 typescript node 模块的教程](https://egghead.io/lessons/typescript-create-high-quality-npm-packages-using-typescript)

Using modules written in TypeScript is super fun as you get great compile time safety and autocomplete (essentially executable documentation).

使用 TypeScript 编写的模块充满乐趣, 因为你可以获得很好的编译时安全与自动完成（相当于可以运行的文档）。

Creating a high quality TypeScript module is simple. Assume the following desired folder structure for your package:

创建一个高质量 TypeScript 模块很简单。假设你的程序包的文件结构如下:

```text
package
├─ package.json
├─ tsconfig.json
├─ src
│  ├─ All your source files
│  ├─ index.ts
│  ├─ foo.ts
│  └─ ...
└─ lib
  ├─ All your compiled files
  ├─ index.d.ts
  ├─ index.js
  ├─ foo.d.ts
  ├─ foo.js
  └─ ...
```


* In your `tsconfig.json`

* 在你的 `tsconfig.json` 里

  * have `compilerOptions`: `"outDir": "lib"` and `"declaration": true` < This generates declaration and js files in the lib folder

  * 有 `compilerOptions `: `"outDir": "lib"` 和 `"declaration": true` < 这会在 lib 目录里生成声明与 js 文件

  * have `include: ["./src/**/*"]` < This includes all the files from the `src` dir.

  * 有 `include: ["./src/**/*"]` < 这会包含 `src` 目录中的所有文件。

* In your `package.json` have

* 在你的 `package.json` 有

  * `"main": "lib/index"` < This tells Node.js to load `lib/index.js`

  *  `"main": "lib/index"` < 这会告诉 Node.js 加载 `lib/index.js`

  * `"types": "lib/index"` < This tells TypeScript to load `lib/index.d.ts`

  *  `"types": "lib/index"` < 这会告诉 TypeScript 加载 `lib/index.d.ts`


Example package:

示例包:

* `npm install typestyle` [for TypeStyle](https://www.npmjs.com/package/typestyle)

*  `npm install typestyle` [安装 TypeStyle](https://www.npmjs.com/package/typestyle)

* Usage: `import { style } from 'typestyle';` will be completely type safe.

*  用法: `import { style } from 'typestyle';` 将会是完全类型安全的。

MORE:

更多:

* If you package depends on other TypeScript authored packages, put them in `dependencies`/`devDependencies`/`peerDependencies` just like you would with raw JS packages.

* 如果你的程序包依赖于其它 TypeScript 编写的包, 把它们像其他原生 JS 包一样放在 `dependencies`/`devDependencies`/`peerDependencies` 里。

* If you package depends on other JavaScript authored packages and you want to use it type safely in your project, put their types e.g. `@types/foo` in `devDependencies`. JavaScript types should be managed *out of bound* from the main NPM streams. The JavaScript ecosystem breaks types without semantic versioning too commonly, so if your users need types for these they should install the `@types/foo` version that works for them.

* 如果你的程序包依赖于其他你想类型安全地使用的 JavaScript 包, 把它们的类型声明（例如`@types/foo`）放进 `devDependencies` 里。JavaScript 类型声明应该在 NPM 主线之外管理。JavaScript 生态经常忽略语义化版本打破类型声明, 所以如果你的用户需要类型声明, 他们应该安装适合自己的 `@types/foo` 版本。

## Bonus points

## 另外一点

Such NPM modules work just fine with browserify (using tsify) or webpack (using ts-loader).

这些 NPM 模块与 browserify（使用 tsify） 或 webpack（使用 ts-loader）配合良好。
