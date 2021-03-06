# The Manifest(清单)

使用webpack构建一个典型应用/网站,这里面有三种类型的代码:

1.  你自己的源代码.
2.  第三方库的代码.

3.webpack的 runtime 和manifest,来指导所有模块的交互.

## Runtime

runtime,处在 manifest data之间, 是webpack所有代码里的基本,当运行在浏览器中的时候,需要连接你的模块化应用.

它包含loading 和 resolving 逻辑 需要连接你的模块作为他们的交流.

这包含正在连接的模块,已经被载入到浏览当中,以及逻辑上的延迟加载那些没有连接的模块.

## Manifest

一旦你的应用在=index.html= 文件形式中,点击浏览器,

一些捆绑包和其他类型的assets会被应用需要,必须被载入和以某种方式链接.

你精心设置的 `src` 目录现在被捆绑,压缩,甚至可能通过webpack的 `optimization` 延迟加载 分割为更小的chunks.

那么webpack如何管理required 模块的彼此之间的交流哪?这就是为什么要谈到 manifest.

```text
    当编译入口,解析,并映射到应用,webpack 在所有的模块上面保持了详细的笔记.
    
    这些收集的信息被称为 "manifest", 在运行时将使用解析和载入模块,一旦它们被捆绑和运送到浏览器当中.
    
    无论你选择哪个模块语法,那些 =import= 或者 =require= 声明 现在变成 =__webpackl_require__= 方法(用于指向模块标识符)
    
    使用manifest中的数据,runtime 将会找出从哪检索标识符后面的模块.
```

## The problem

现在你有一点关于webpack场景背后工作的一些见解.

但是它跟自己有毛关系哪?

`runtime webpack` 做自己的事情,运用清单,每件事都将会发生,一旦你的应用点击了浏览器.

如果你想要提高你的项目的性能,比如通过运用浏览器缓存,这个进程突然成为一个需要理解的重要事情.

通过在你的 捆绑文件名中 运用content hash,当文件内容发生改变的时候,你可以指示浏览器缓存无效.

一旦你开始使用,你将立即注意到这些有趣的行为.

即便他们的内容没有明显的改变,某些hashes的也会改变. 这是由runtime的注入和每次构建改变的manifest 造成的 .

TODO:
1. extract the manifest
2. caching

