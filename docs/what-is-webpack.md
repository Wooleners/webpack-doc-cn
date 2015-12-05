**webpack** 是一个 **模块打包工具**.

webpack 通过依赖获取模块并且为这些模块生成静态资源

![modules with dependencies ---webpack---> static assets](http://webpack.github.io/assets/what-is-webpack.png)

## 我什么不是别的模块打包工具?

现有的模块打包工具不能很好的适用于大学项目(大型的单页程序). 开发webpack的最重要的原因是代码分割和静态资源应该通过模块化完美的结合在一起

我尝试过扩展现有的模块打包工具, 但是还是不能实现所有需求

## 目标

* 依赖树拆分为按需加载块
* 保持最低的初始化加载时间
* 每个静态资源应该被当作一个模块
* 第三方库集成模块的能力
* 打包几乎所有的自定义功能模块
* 适合大项目

## webpack有何不同?

#### [Code Splitting](http://webpack.github.io/docs/code-splitting.html)

webpack在依赖树中有两种类型的依赖：同步和异步。
异步依赖作为分割点，形成了一个新的块。
在数据块树优化以后，每个块的文件被喷出。

更多关于 [Code Splitting](http://webpack.github.io/docs/code-splitting.html)

#### [Loaders]

webpack 只可以被用来处理JavaScript, 但是 loaders 能够把其它资源转换为JavaScript. 这样,每个资源都是一个模块.

更多关于 [使用 loaders](http://webpack.github.io/docs/using-loaders.html) and [Loaders](http://webpack.github.io/docs/loaders.html).

#### 智能解析

webpack有一个智能的解析器，它能处理几乎每个第三方库.它甚至允许依赖项中的表达式，例如 `require("./templates/" + name + ".jade")`. 它可以处理最常见的模块样式: [CommonJs] and [AMD].

更多关于 [expressions in dependencies | Context](http://webpack.github.io/docs/context.html)

#### 插件系统

webpack功能丰富的插件系统. 最内部的特征都是基于这个插件系统. 这使您能够根据您的需要定制webpack发布常见的开源插件。.

更多关于 [Plugins](http://webpack.github.io/docs/plugins.html).
