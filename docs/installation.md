## node.js

安装 [node.js](http://nodejs.org).

node.js 配备了一个叫 `npm` 的软件包管理器.

## webpack

webpack 能够通过 `npm` 安装:

``` sh
$ npm install webpack -g
```

webpack现在安装在全局，而且webpack命令已经生效了

## 在项目中使用webpack

最好在你的项目中也把webpack作为依赖. 通过这个你可以选择一个本地webpack版本而不会强制使用单一的全局版本.

添加一个 **package.json** 配置文件

``` sh
$ npm init
```

如果你不想发布你的项目到npm,可以略过

安装webpack

``` sh
$ npm install webpack --save-dev
```

## 版本

有2个版本的webpack， stable版本和beta版本。beta版本通过 **-beta** 安装。
beta版本可能包含不可靠的修改或实验的特性，并且是缺乏测试的.正式的项目你应该使用stable版本

``` sh
$ npm install webpack@1.2.x --save-dev
```

## Dev Tools
如果你想使用 dev tools
``` sh
$ npm install webpack-dev-server --save-dev
```
