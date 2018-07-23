## Truthy

## 真值

JavaScript has a concept of `truthy` i.e. things that evaluate like `true` would in certain positions (e.g. `if` conditions and the boolean `&&` `||` operators). The following things are truthy in JavaScript. An example is any number other than `0` e.g.

JavaScript有`truthy`的概念，即在某种状态下值等于`true`（例如`if`条件和布尔`&&``||`运算符）。以下是在JavaScript中为真值的内容。例如，除`0`以外的任何数字。

```ts
if (123) { // Will be treated like `true`
  console.log('Any number other than 0 is truthy');
}
```

Something that isn't truthy is called `falsy`.

非真值我们称之为`falsy`。

Here's a handy table for your reference.

下表可供你参考。

| Variable Type   | When it is *falsy*       | When it is *truthy*      |
|-----------------|--------------------------|--------------------------|
| `boolean`       | `false`                  | `true`                   |
| `string`        | `''` (empty string)      | any other string         |
| `number`        | `0`  `NaN`               | any other number         |
| `null`          | always                   | never                    |
| `undefined`     | always                   | never                    |
| Any other Object including empty ones like `{}`,`[]` | never | always |


### Being explicit

### 明确

> The `!!` pattern

> `!!`模式

Quite commonly it helps to be explicit that the intent is to treat the value as a `boolean` and convert it into a *true boolean* (one of `true`|`false`). You can easily convert values to a true boolean by prefixing it with `!!` e.g. `!!foo`. Its just `!` used *twice*. The first `!` converts the variable (in this case `foo`) to a boolean but inverts the logic (*truthy* -`!`> `false`, *falsy* -`!`> `true`). The second one toggles it again to match the nature of the original object (e.g. *truthy* -`!`> `false` -`!`> `true`).

通常，明确意图有助于将值当作`boolean`并将其转换为*true boolean*(`true`|`false`其中之一)。你可以通过在前面加`!!`来轻易地将值转换为真的布尔值，例如`!!foo`。仅仅使用两次`!`。第一个`!`将变量（本例中为`foo`）转换为布尔值，但会反转逻辑(*truthy* -`!`> `false`, *falsy* -`!`> `true`)。第二个`!`再次切换它以匹配原始对象的本质，例如*truthy* -`!`> `false` -`!`> `true`。

It is common to use this pattern in lots of places e.g.

在多数情况下使用这种模式是很常见的，如下

```ts
// Direct variables
const hasName = !!name;

// As members of objects
const someObj = {
  hasName: !!name
}

// e.g. ReactJS
{!!someName && <div>{someName}</div>}
```
