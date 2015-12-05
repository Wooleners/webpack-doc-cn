随着网站逐渐演化为Webapp

* 页面中越来越多的JavaScript代码
* 在现代浏览器中我们可以做的越来越多
* 更多的无刷新需求 → 意味着更多的前端代码

因此前端代码会 **越来越多，越来越庞大**

一个大型的代码库需要被规划. 模块化系统是切分代码库的一种选择.

# 模块化系统风格

有多个标准制定了如何去定义依赖项和输出项

* `<script>` 标签风格(去烧模块系统)
* CommonJs
* AMD 和它的各种方言
* ES6 Modules
* 还有很多

## `<script>` 标签风格

如果你不使用一个模块系统,一般你会这样处理一个模块化的代码库

```html
<script src="module1.js"></script>
<script src="module2.js"></script>
<script src="libraryA.js"></script>
<script src="module3.js"></script>
```
模块会定义一个全局变量用来暴露接口,模块的接口可以访问全局对象的依赖关系

常见问题

* 全局变量冲突
* 加载的顺序是很重要的
* 开发人员必须解决模块/库的依赖关系
* 在大型项目中去维护这个资源列表是十分困难的

## CommonJs: 同步加载

CommonJs 使用同步的 **require** 方法去加载依赖并返回一个输出接口. 一个模块能够通过在 **exports** 上添加属性或者重新设置 **module.exports** 的值来特殊输出, 例如下面的nodejs代码

```javascript
require("module");
require("../file.js");
exports.doStuff = function() {};
module.exports = someValue;
```

###优点

* 前后端代码复用
* npm生态圈庞大
* 使用极其简单

###缺点

* 浏览器加载脚本天生不支持同步的加载
* 不能平行加载多个模块

###实现

* [node.js](http://nodejs.org/) - server-side
* [browserify](https://github.com/substack/node-browserify)
* [modules-webmake](https://github.com/medikoo/modules-webmake) - compile to one bundle
* [wreq](https://github.com/substack/wreq) - client-side

## AMD: 异步加载

[`异步模块定义`](https://github.com/amdjs/amdjs-api/wiki/AMD)

其他的基于浏览器的模块系统为了解决CommonJs的同步问题,引进了一套异步版本

``` javascript
require(["module", "../file"], function(module, file) { /* ... */ });
define("mymodule", ["dep1", "dep2"], function(d1, d2) {
  return someExportedValue;
});
```

#### 优点

* 支持网络的异步请求
* 支持平行加载多个模块

#### 缺点

* 由于依赖需要前置，意味着更大的开发成本
* 似乎是某种形式的变通方案

#### 实现

* [require.js](http://requirejs.org/) - client-side
* [curl](https://github.com/cujojs/curl) - client-side

更多关于 [[CommonJs]] and [[AMD]].

## ES6 modules

EcmaScript6 给JavaScript添加了一些新的语言特性,形成的另一个模块系统。

``` javascript
import "jquery";
export function doStuff() {}
module "localModule" {}
```

#### 优点

* 静态分析非常简单
* 不会过时的ES标准

#### 缺点

* 浏览器支持还需要时间
* 很少有模块支持

## 公正的解决方案

给开发人员选择模块的风格。兼容现有的代码。可以很容易地添加自定义模块风格。

---

# 传输

模块应该在客户端执行, 所以他们必须从服务器转移到浏览器。有两种极端如何传输模块:

* 每个模块一个请求
* 所有模块一个请求

都是可行的, 但是它们都不是最优的:

* 每个模块一个请求
    * 优点: 仅仅调用的模块才会被请求
    * 缺点: 许多请求意味着很多开销
    * 缺点: 因为有网络延迟，应用启动缓慢
* 所有模块一个请求
    * 优点: 请求的开销更少,更少的延迟
    * 缺点: 没有调用的模块也被请求了

## 分块传输

更灵活的传输效果会更好。这两个极端之间的折中在大多数情况下会更好。
→当编译所有模块：拆分主模块为多个较小的块。
我们得到多个较小的请求。所有的块只有在需要的时候才会加载。最初的请求不包含你的完整代码库。所以他是很小的.
而“分割点”是可以由开发人员可选的。
→一个大的代码库是可能的！

Note: The [idea is from Google's GWT](https://developers.google.com/web-toolkit/doc/latest/DevGuideCodeSplitting).

更多关于 [Code Splitting](http://webpack.github.io/docs/code-splitting.html).

---

# 为什么只有 JavaScript?

我什么需要一个模块系统来帮助JavaScript开发者? 因为有大量其它的静态资源需要处理

* 样式
* 图片
* webfonts
* HTML模版
* etc.

并且:

* coffeescript → javascript
* elm → javascript
* less stylesheets → css stylesheets
* jade templates → javascript which generates html
* i18n files → something
* etc.

这也是一样简单

``` javascript
require("./style.css");
```

``` javascript
require("./style.less");
require("./template.jade");
require("./image.png");
```

更多关于 [Using loaders](http://webpack.github.io/docs/using-loaders.html) and [Loaders](http://webpack.github.io/docs/loaders.html).

---

# 静态解析

当编译所有的模块时,静态分析会尝试去寻找依赖,通常我们可以不用表达式找到简单的东西, 但是它甚至允许依赖项中的表达式，例如 `require("./template/" + templateName + ".jade")` .

许多库使用不同的风格，其中一些是非常古怪的

## Strategy

一个聪明的解析器将允许大多数现有的代码运行. 如果开发人员做了一些奇怪的事情它会尝试找到最适合的解决方案。
