# webpack学习

构建工具的一种，`静态模块打包器`，，模块处理

chunk：代码块

打包：将代码块经过某种处理

bundle:打包后输出的文件（对应的静态资源）

## 5个核心概念

### 1.Entry

入口指示

### 2.Output

输出指示

### 3.Loader

让webpack去处理非javascript文件（webpack自身只理解javascript）

翻译

### 4.Plugins

插件的操作

### 5.Mode

模式指示

## npm命令

```js
npm init

npm i webpack webpack-cli -g

npm i webpack webpack-cli -D


// webpack入口文件 index.js
//开发环境： webpack ./src/index.js -o ./build/built.js --mode=development
//开发环境： webpack ./src/index.js -o ./build/built.js --mode=production
 
//webpack能处理js/json资源，不能处理css/img等资源
//生产环境和开发环境将ES6模块化编译成浏览器能识别的模块化
//生产环境比开发环境多一个压缩js文件
```

