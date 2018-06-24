## Compilation Context
The compilation context is basically just a fancy term for grouping of the files that TypeScript will parse and analyze to determine what is valid and what isn't. Along with the information about which files, the compilation context contains information about *which compiler options* are in use. A great way to define this logical grouping (we also like to use the term *project*) is using a `tsconfig.json` file.

## 编译上下文

The compilation context is basically just a fancy term for grouping of the files that TypeScript will parse and analyze to determine what is valid and what isn't.
编译上下文只是很基本的一个术语，会被用在对文件的分组整理过程中，TypeScript 会对这些文件进行理解和分析，来确定哪些是需要用到的，哪些不是。

Along with the information about which files, the compilation context contains information about *which compiler options* are in use.
通过从这些文件当中整理出来的信息，编译上下文会得到一个有关于 *哪些编译选项* 正在被使用的信息（译者注：配置喽），并且把这个信息保存起来。

A great way to define this logical grouping (we also like to use the term *project*) is using a `tsconfig.json` file.
有一个定义逻辑分组的推荐做法（译者注：推荐配置喽），可以使用 `tsconfig.json` 文件进行配置。