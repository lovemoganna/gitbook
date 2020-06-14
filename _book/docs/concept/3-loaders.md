# Loaders

Loaders 是一个转换器.被应用在一个模块的源代码上.

它们允许你 `预处理` pro-process 文件作为你 `import` or `load` them.

因此, `loaders` 是一个像 处理 前端构建步骤的 `"tasks"`.

`Loaders` 可以转换来自不同语言的 files 成为JS.也可以载入内联图片作为 `data URLS`.

`Loaders` 甚至允许你做一些事情,比如: 直接从你的JS模块里面 `import` CSS files.

## Example

可以告诉webpack 载入 css 文件,或者转换typescript 成为 JavaScript.

要想做这个你得先安装loaders.
```js
    npm install --save-dev css-loader ts-loader
```

然后,就会通知webpack 对每个 `.css` 文件来使用 `css-loader` 了.

这种的话,需要写规则.

```js
    //webpack.config.js
    //------------------------------------------
      module.exports = {
        module: {
          rules: [
    	{ test: /\.css$/, use: 'css-loader' },
    	{ test: /\.ts$/, use: 'ts-loader' }
          ]
        }
      };
```
## Using Loaders

有3个法来使用loaders.

-   Configuration: 直接在 `webpack.config.js` 里面配置就可以.
-   Inline: 这种的话,需要在每个 `import` 声明中明确指定.
-   CLI: 命令行操作.

## Configuration

`module.rules` 允许你在你的webpack配置当中 指定一些loaders.

这是一种展示 loaders的简式方法,可以帮助你维护干净的代码.它也为你提供了各个loader的全部预览.

Loaders `从右到左/从下到上` 被评估/执行,

比如,先从 `sass-loader`,继续从 `css-loader` 执行,最终是 `style-loader` .

```js
    module.exports = {
      module: {
        rules: [
          {
    	test: /\.css$/,
    	use: [
    	  // style-loader
    	  { loader: 'style-loader' },
    	  // css-loader
    	  {
    	    loader: 'css-loader',
    	    options: {
    	      modules: true
    	    }
    	  },
    	  // sass-loader
    	  { loader: 'sass-loader' }
    	]
          }
        ]
      }
    };
```
INLINE and CLI 的模式先不用了.没必要搞个麻烦的例子.

## Resolving Loaders

loaders 使用 npm 来管理.
