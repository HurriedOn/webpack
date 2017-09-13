## 输出文件的文件名
通过使用 output.filename 进行文件名替换，可以确保浏览器获取到修改后的文件。使用 [chunkhash] 替换，在文件名中包含一个 chunk 相关(chunk-specific)的哈希。

项目结构：
* webpackDemo
    * package.json
    * webpack.config.js
    * dist
    * src
        * index.js
    * node_modules


webpack.config.js：
```js
const path = require('path');
const CleanWebpackPlugin=require('clean-webpack-plugin');
const HTMLWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  plugins: [
  	new CleanWebpackPlugin(['dist']),
    new HTMLWebpackPlugin({
      title: 'Caching'
    })
  ],
  output: {
    filename: '[name].[chunkhash].js',
    path: path.resolve(__dirname, 'dist')
  }
};
```
src/index.js：
```js
import _ from 'lodash';

function component() {
	var element = document.createElement('div');

  element.innerHTML = _.join(['Hello', 'webpack T'], ' ');

  return element;
}

document.body.appendChild(component());
```
运行命令：
```dash
$ webpack
```
如果我们不做修改，然后再次运行构建，我们的文件名按照期望，依然保持不变。然而，如果我们修改内容再次运行，文件名会改变。
