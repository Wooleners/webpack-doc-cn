# loaders 是什么?

Loaders 是用来转换你app中应用的资源. 它们是能够把资源文件作为参数并返回新的资源的一些方法 (运行在node.js)

例如,你能够使用loaders告诉webpack去加载coffeeScript和JSX文件

## Loader 特性

* Loaders 可以链接使用. 它们被应用在资源流中. 最后的loader将返回JavaScript，其它的可以返回任意格式（传递到下一个loader）
* Loaders 可以同步也可以异步.
* Loaders 运行在node.js， 只要nodejs可以做的，loaders都是可能的.
* Loaders 接受参数. 这可用于配置loaders
* Loaders 可以在配置中绑定 extension／正则
* Loaders 可以通过npm安装发布
* 正常的模块可以输出一个loader 除了这个loader是在package.json中定义的loader
* Loaders 可配置.
* Plugins 可以给loaders带来更多特性.
* Loaders 可以生成更多的任意类型的文件.
* [[etc. | loaders]]

如果你对loader感兴趣， 这里是一些loader的例子 [list of loaders](http://webpack.github.io/docs/list-of-loaders.html).

# loaders 解析

loaders是用来处理相似的模块.一个loader模块预期输出一个要被写入兼容nodejs的方法. 在通常情况下你可以用npm管理，但你也可以有把loaders作为文件放在你的app里.

## 参考 loaders

按照惯例, 尽管不要求, loaders 通常被称为 `XXX-loader`, `XXX` 是它处理文件流的名字. 例如, `json-loader`.

你可以通过完整的名称来引用 (例如 `json-loader`), 也可以使用它们的简写名称 (例如 `json`).

loader 命名规则和优先级搜索顺序由webpack配置API中的 [`resolveLoader.moduleTemplates`](http://webpack.github.io/docs/configuration.html#resolveloader-moduletemplates) 定义
loader名称约定可能是有用的，尤其是在 需要引用它们的时候。请参阅下面的用法

## 安装 loaders

如果npm上存在此loader,你可以通过npm 安装

``` sh
$ npm install xxx-loader --save
```

or

``` sh
$ npm install xxx-loader --save-dev
```
# 用法

在你的app中有多种方法去使用loaders

* 使用 `require`
* 通过 config
* 通过 CLI

## loaders in `require`

> **注意:** 如果可能的话，避免使用这一点。如果你的脚步无关环境（node.js中和浏览器），建议使用配置约定指定loader（见下节）。

可以在需要声明的地方(`require`)指定loaders (或 `define`, `require.ensure`, 等等). 用 `!`从资源分割开loaders. 每个部分会相对于当前目录被解析

``` javascript
require("./loader!./dir/file.txt");
// => 使用当前目录下的loader.js去转换
//    "file.txt" 在 "dir" 文件夹下.

require("jade!./template.jade");
// => 使用 "jade-loader" (并且jade是通过npm被安装在node_modules下)
//   转换当前目录下的 "template.jade"

require("!style!css!less!bootstrap/less/bootstrap.less");
// => 文件 "bootstrap.less" 在 "bootstrap/less"
//   会被less-loader,css-loader,style-loader从左到右依次处理
```


## 配置

你可以通过配置给loaders绑定正则

``` javascript
{
	module: {
		loaders: [
			{ test: /\.jade$/, loader: "jade" },
			// => "jade" loader 被使用于 ".jade" 文件

			{ test: /\.css$/, loader: "style!css" },
			// => "style" 和 "css" loader 被用于 ".css" 文件
			// 可选:
			{ test: /\.css$/, loaders: ["style", "css"] },
		]
	}
}
```

## CLI

你可以通过CLI 给loaders绑定extension:

``` sh
$ webpack --module-bind jade --module-bind 'css=style!css'
```

这里是用 loader "jade" 处理 ".jade" 文件
loaders "style" 和 "css" 处理 ".css" 文件.

## Query parameters

Loader可以传递通过查询字符串查询参数（就像在web上一样）。查询字符串被附加到loader `?` 后面。例如 `url-loader?mimetype=image/png`

注意: 查询字符串的格式取决于loader. 查看格式在 loader的文档. 绝大部分loader接受正常的查询格式参数 例如 (`?key=value&key2=value2`) 或则是一个 JSON 对象 (`?{"key":"value","key2":"value2"}`).

### in `require`

``` javascript
require("url-loader?mimetype=image/png!./file.png");
```

### 配置

``` javascript
{ test: /\.png$/, loader: "url-loader?mimetype=image/png" }
```

或则

``` javascript
{
	test: /\.png$/,
	loader: "url-loader",
	query: { mimetype: "image/png" }
}
```


### CLI

``` sh
webpack --module-bind "png=url-loader?mimetype=image/png"
```
