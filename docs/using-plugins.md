## 内置插件

插件通过在WebPack配置插件属性来包含在你的模块中。

``` javascript
var webpack = require("webpack");

module.exports = {
	plugins: [
		new webpack.ResolverPlugin([
			new webpack.ResolverPlugin.DirectoryDescriptionFilePlugin("bower.json", ["main"])
		], ["normal", "loader"])
	]
};
```

## 其它插件

可以通过npm安装npm上存在的非内置的插件

``` text
npm install component-webpack-plugin
```

然后就可以使用了

``` javascript
var ComponentPlugin = require("component-webpack-plugin");
module.exports = {
	plugins: [
		new ComponentPlugin()
	]
}
```

当我们使用npm安装第三方插件，npm建议我们使用这个工具
[https://www.npmjs.com/package/webpack-load-plugins](https://www.npmjs.com/package/webpack-load-plugins)

它能够检查所有的第三方插件，安装它们，并且按需加载

## 参见

* [list of plugins](http://webpack.github.io/docs/list-of-plugins.html)
