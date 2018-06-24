## Which files?

You can either use `files` to be explicit:

```json
{
    "files":[
        "./some/file.ts"
    ]
}
```

or `include` and `exclude` to specify files / folders / globs. E.g.:


```json
{
    "include":[
        "./folder"
    ],
    "exclude":[
        "./folder/**/*.spec.ts",
        "./folder/someSubFolder"
    ]
}
```

Some notes:

* For globs : `**/*` (e.g. sample usage `somefolder/**/*`) means all folder and any files (the extensions `.ts`/`.tsx` will be assumed and if `allowJs:true` so will `.js`/`.jsx`)

## Which files?
## 都有哪些文件呢?

You can either use `files` to be explicit:
一种通常的情况，你可以明确的指定需要使用的文件：

```json
{
    "files":[
        "./some/file.ts"
    ]
}
```

or `include` and `exclude` to specify files / folders / globs. E.g.:
还有一种情况，需要使用的文件在一些情况下并不是绝对明确的，可以使用 `include` 和 `excloud` 来配置 files / folders / globs。例如：

```json
{
    "include":[
        "./folder"
    ],
    "exclude":[
        "./folder/**/*.spec.ts",
        "./folder/someSubFolder"
    ]
}
```

Some notes:
一点注意事项：

* For globs : `**/*` (e.g. sample usage `somefolder/**/*`) means all folder and any files (the extensions `.ts`/`.tsx` will be assumed and if `allowJs:true` so will `.js`/`.jsx`)
* 在使用 globs 的时候 ： `**/*` （e.g. 简单的使用方式 `somefolder/**/*`） 意味着会匹配到所有的文件夹， 和文件夹下面的所有文件 （默认的拓展名是 `.ts`/`.tsx`，如果配置了 `allowJs:true`， 拓展名则会更改为 `.js`/`.jsx` ）