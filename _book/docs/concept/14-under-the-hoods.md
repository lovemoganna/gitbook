# under the hooks

捆绑过程是一个带来一些文件和发射其他文件的函数.

## the mainparts

-  index.js
-  app.js

要是用它们,模块来自 `ModuleGraph`.

在捆绑进程期间,模块包含在`chunks`里面.

`Chunks`包含在来自 通过模块互相连接 `ChunkGraph` 的 `Chunk groups` 里面.

当你描述一个入口点的时候,在`hood`之下,你创建了带有`chunk`的 `chunk group`.

## Chunks

-   initial:  `entry: index.jsx / index.js`  | `output: main.js` | `output.filename`
-   non-initial: `output.chunkFilename`
- [name]
- [contenthash]

