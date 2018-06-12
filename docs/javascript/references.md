## References

## 引用

Beyond literals, any Object in JavaScript (including functions, arrays, regexp etc) are references. This means the following

除文字外，JavaScript中的任何对象（包括函数，数组，regexp等）都是引用。这意味着以下内容

### Mutations are across all references

### 所有参考都有突变

```js
var foo = {};
var bar = foo; // bar is a reference to the same object

foo.baz = 123;
console.log(bar.baz); // 123
```

### Equality is for references

### 平等是为了参考

```js
var foo = {};
var bar = foo; // bar is a reference
var baz = {}; // baz is a *new object* distinct from `foo`

console.log(foo === bar); // true
console.log(foo === baz); // false
```
