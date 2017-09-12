有三种常用的代码分离方法：

+ 入口起点：使用 entry 配置手动地分离代码。
+ 防止重复：使用 CommonsChunkPlugin 去重和分离 chunk。
+ 动态导入：通过模块的内联函数调用来分离代码。

## 入口起点
目录：
* webpackDemo
    * package.json
    * webpack.config.js
    * dist
    * src
        * index.js
        * another-module.js
    * node_modules

another-module.js：
```js
import _ from 'lodash';

console.log(
  _.join(['Another', 'module', 'loaded!'], ' ')
);
```
webpack.config.js：
```js
const path = require('path');
const HTMLWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: {
    index: './src/index.js',
    another: './src/another-module.js'
  },
  plugins: [
    new HTMLWebpackPlugin({
      title: 'Code Splitting'
    })
  ],
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```
执行命令：
```bash
$ webpack
```
这种方法存在一些问题:
+ 如果入口 chunks 之间包含重复的模块，那些重复模块都会被引入到各个 bundle 中。
+ 这种方法不够灵活，并且不能将核心应用程序逻辑进行动态拆分代码。
接着，我们通过使用 CommonsChunkPlugin 来移除重复的模块
## 防止重复
webpack.config.js：
```js
const path = require('path');
const webpack=require('webpack');
const HTMLWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: {
    index: './src/index.js',
    another: './src/another-module.js'
  },
  plugins: [
    new HTMLWebpackPlugin({
      title: 'Code Splitting'
    }),
    new webpack.optimize.CommonsChunkPlugin({
    	name:'common'
    })
  ],
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```
执行命令：
```bash
$ webpack
```
