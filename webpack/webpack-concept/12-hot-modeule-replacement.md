# Hot module replacement

Hot Module Replacement (HMR) exchange,adds,or removes module while an applications is running, without a full reload.

This can sinigicantly speed up development in a few ways:

-   保持全部重载期间丢失的应用状态
-   更新改变的部分来节省宝贵的开发时间
-   当修改的部分放到源代码中的CSS/JS 的时候,浏览器立即更新.这个效果就像在浏览器 dev tools 直接改变style一样.

## How It works
## In the application

下面的步骤允许模块在应用之外交流:

1.  应用询问HMR runtime 检查更新.
2.  runtime 异步下载更新并通知应用.
3.  应用接下来询问 runtime 来apply 更新内容.
4.  runtime 同步应用更新.

你可以设置HMR 以便于这个进程自动发生,对于更新,你也可以选择 require 用户注入来产生.

## In the compiler

除了正常的 assets之外,编译器需要发出一个 "update" 来告诉先前版本到新版本允许更新.

这个 "update" 由 两部分组成:

1. Updated `manifest` (JSON)
2. 多个update chunk (JS)

这个 manifest 包含新的 compilation hash 和 所有update chunks的list.

每个chunk都含有全部更新模块的新代码.(或者是一个标识,来指示哪些模块被移除)

编译器确保模块的ID和chunk的IDs 和这些构建是一致的.通常编译器通常把这些IDs 存在内存当中.比如说(webpack-dev-server),但是也可能存在JSON file 当中.

## In a Module

HMR是一个可选的功能,仅影响包含HMR code的模块.

一个很好的例子就是通过 `style-loader` 打补丁styling.

要想这个补丁正常工作, `style-loader` 实现了 HMR的 接口,当它通过HMR 接收到 update的时候,它将会用新的样式来替换旧的样式.

类似的,当在一个模块实现HMR接口的时候,你可以描述当模块更新的时候,什么应该发生.

但是,大多数情况下,并不强制在每个模块中写下HMR的代码.

如果一个模块没有HMR handlers,这个update就会略过.

这意味着单个 handler 可以更新一个完整的模块树.如果来自这个树的单个模块更新了,那么整个依赖集就会被重载.

## In the Runtime

对于模块系统的runtime,发出额外的代码来追踪 模块 `parents` 和 `children`.在管理方面,这个runtime支持两个方法: `check` 和 `apply`.

`check` 让 http请求 来更新 `manifest`.如果请求失败了,更新无效.如果成功了,更新的chunks列表是不同于当前已经载入的chunks了.

对于每个已经载入的chunk,对应的更新chunk会被下载.

所有模块更新在runtime期间被存储.当所有更新的chunk下载完毕并准备应用的时候,runtime会跳转为 `ready` state.

`apply` 方法标志所有更新的模块无效.对于每个无效的模块,它们需要在模块或者父级模块里面更新 `handler`.

否则,无效标识将会省略并且无视父级模块.每个都会省略,直到APP的入口点或者模块收到带有更新 `handler`.

如果从一个入口点被省略,那么这个进程就GG了.

之后所有无效的模块都会被处理(包含处理handler) 和 卸载.

当前的hash接下来会更新并且全部 `accept` 被调用的handlers.

runtime 会跳转回 `idle` 状态,一切都将正常进行.

