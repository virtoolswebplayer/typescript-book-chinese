## References

## 引用 References

Beyond literals, any Object in JavaScript (including functions, arrays, regexp etc) are references. This means the following

除文字外，JavaScript 中的任何对象（包括函数，数组，regexp 等）都是引用。这意味着以下内容

### Mutations are across all references

### 所有引用都有突变

```js
var foo = {};
var bar = foo; // bar 是对同一个对象的引用

foo.baz = 123;
console.log(bar.baz); // 123
```

### Equality is for references

### 相等其实就是引用相同

```js
var foo = {};
var bar = foo; // bar 是一个引用
var baz = {};  // baz 是与foo不同的 *全新对象*`

console.log(foo === bar); // true 因为foo 和 bar 引用相同
console.log(foo === baz); // false
```
