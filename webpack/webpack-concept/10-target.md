# Target

因为JS可以对所有服务器和浏览器写入,webpack提供了多个部署对象,你可以在你的webpack 配置里面设置.

webpack target 属性不能和 `output.libraryTarget` 属性相混淆.

# Usage

要想设置 `target` 属性,需要在webpack config 里面简单设置 target 值:

```js
    module.exports = {
      target: 'node'
    };
```

使用 `node` webpack将会在NODEJS 环境下 编译使用.

(使用 node.js 的require 将会载入 chunks 并不会触及任何内置于模块的内容. 比如: fs / path)

每个 target 有多重 部署/环境指定附加项,满足它的需要.

# Multi Targets

尽管webpack 不支持多行字符串 来传递 `target` 属性,你可以创建一个 同构(isomorphic) 库,使用两个分离的配置就可以做到:

```js
    const path = require('path');
    const serverConfig = {
      target: 'node',
      output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'lib.node.js'
      }
      //…
    };
    
    const clientConfig = {
      target: 'web', // <=== can be omitted as default is 'web'
      output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'lib.js'
      }
      //…
    };
    
    module.exports = [ serverConfig, clientConfig ];
```

上面的这个例子将会在 `dist` 文件中 创建 `lib.js` 和 `lib.node.js` .

# Resources

还有很多部署对象供你选择,你可以看下下面的例子

-   compare-webpack-target-bundles
-   boilerplate of electron - react application

