# module resolution

一个解析器是一个库.通过绝对路径来定位模块.

```js
    import foo from 'path/to/module';
    // or
    require('path/to/module');

```

这个依赖模块可能来自应用代码,也可能来自第三方库.

解析器帮助webpack发现模块的代码,对于每一个 `require/import` 声明需要被包含在捆绑包当中.

在捆绑模块的时候,webpack 使用 `enhanced-resolve` 来解析文件路径

## resolving rules in webpack

Using `enhanced-resolve` , webpack可以解析三种类型的文件路径.

-   Absolute Paths

```js
    import '/home/me/file';
    
    import 'C:\\Users\\me\\file';

```
-   Relative Paths

```js
    import '../src/file1';
    import './file2';
```

-   Module paths

```js
    import 'module';
    import 'module/lib/file';
```

模块会搜索 所有由 `resolve.modules`  指定的目录.

你可以替换 `origin module` 路径,只需要通过一个备用路径: 使用 `resolve.alias` 配置选项创建一个alias.

一旦路径基于上面的规则被解析,解析器会检查看这个 `path point` 是一个文件还是目录,如果 `path point` 是一个文件:

-   如果path 是一个文件扩展,那这个文件会直接被捆绑.
-   如果这个 file extension 使用 `resolve.extension` 选项来进行解析,这将告诉解析器,这个 `extensions` 被解析过程所认同.比如说: `.js /.jsx`

如果 path point 是一个文件夹,下面的一步一步是在正确的extension中发现正确的file.

-   如果folder包含一个 `package.json` 文件,接下来这个字段通过指定=resolve.mainfields= 配置选项来有序查看,第一个字段 `package.json` 明确文件路径.

-   如果没有 `package.json` 文件,或者 `resolve.mainField` 没能返回一个有效的路径, `resolve.mainFiles` 配置选项中指定的file name会被有序查看, 主要看下 `importted/required` 的目录里,正在匹配的文件名是否存在.

-   file extension 以同样的方式使用 `resolve.extensions` 选项被解析.

webpack 根据你的构建对象提供合理的选项.

## Resolving Loaders

对文件解析的同种规则也是由其所指定.但是 这个 `resolveLoader` 配置选项可以对 `loaders` 用于分离解析规则.

## Caching

每一个文件系统的访问被缓存,所以对同一个文件的多个并行或连续请求响应更快.

在 `watch mode` ,只有被修改的文件会从缓存中被剔除.如果 `watch mode` 关闭了,这些缓存将会在每次编译之前被清除.

可以看下 Resolve API 来了解更多 `mentioned above` 的配置选项.

