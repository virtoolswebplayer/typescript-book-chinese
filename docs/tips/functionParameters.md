# Function Parameters

# 函数参数

If you have a function that takes too many parameters, or parameters of the same type, then you might want to consider changing the function to take an object instead.

如果你的函数需要太多参数或者相同类型的参数，那么你可能需要考虑改变函数用对象来替代函数参数。

Consider the following function:

思考以下函数：

```ts
function foo(flagA: boolean, flagB: boolean) {
  // your awesome function body 
}
```

With such a function definition it's quite easy to invoke it incorrectly e.g. `foo(flagB, flagA)` and you would get no help from the compiler. 

使用这样的函数定义，你容易错误的调用函数例如`foo(flagB,flagA)`，并且编译器不会帮助你识别错误。

Instead, convert the function to take an object:

相反的，你使用对象来代替函数参数：

```ts
function foo(config: {flagA: boolean, flagB: boolean}) {
  const {flagA, flagB} = config;
  // your awesome function body 
}
```

Now the function calls will look like `foo({flagA, flagB})` which makes it much easier to spot mistakes and code review.

现在，像`foo({flagA, flagB})`这样调用函数，可以更容易的发现代码错误和有利于代码审查。

> Note : If your function is simple enough, and you don't expect much churn, then feel free to ignore this advice 🌹.

> 注意：如果你的函数很简单，并且你不期望很多流失（churn此处待修改），那么请忽略此条建议。
