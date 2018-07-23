## JQuery Tips

## JQuery 提示

Note: you need to install the `jquery.d.ts` file for these tips

注意: 你需要为这些提示安装jquery.d.ts文件

### Quickly define a new plugin

### 快速定义新插件

Just create `jquery-foo.d.ts` with:

仅需使用下文来创建jquery-foo.d.ts即可

```ts
interface JQuery {
  foo: any;
}
```

And now you can use `$('something').foo({whateverYouWant:'hello jquery plugin'})`

此时你可以这样`$('something').foo({whateverYouWant:'hello jquery plugin'})`使用jQuery的语法