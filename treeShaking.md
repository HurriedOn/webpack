## Tree Shaking
tree shaking 是一个术语，通常用于描述移除 JavaScript 上下文中的未引用代码(dead-code)。它依赖于 ES2015 模块系统中的静态结构特性，例如 import 和 export。
### 添加一个通用模块
新添加文件src/math.js：
* webpackDeom
    * package.json
    * webpack.config.js
    * dist
        * index.html
        * bundle.js
    * src
        * index.js
        * math.js
    * node_modules
src/math.js：
```js
export function square(x) {
  return x * x;
}

export function cube(x) {
  return x * x * x;
}
```
src/index.js：
```js
import { cube } from './math.js';
  function component() {
    var element = document.createElement('pre');
    element.innerHTML = [
      'Hello webpack!',
      '5 cubed is equal to ' + cube(5)
    ].join('\n\n');

    return element;
  }
document.body.appendChild(component());
```
运行命令：
```bash
$ webpack
```
检验dist/bundle.js中是否包含：
```js
function square(x) {
  return x * x;
}

function cube(x) {
  return x * x * x;
}
```
### 精简输出
我们已经通过 import and export 语法，标识出了那些“未引用代码(dead code)”，添加一个能够删除未引用代码的压缩工具：
```bash
$ npm i --save-dev uglifyjs-webpack-plugin
```
webpack.config.js：
```js
const path = require('path');
const UglifyJSPlugin = require('uglifyjs-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  plugins: [
    new UglifyJSPlugin()
  ]
};
```
运行命令：
```bash
$ webpack
```
现在整个 bundle 都已经被精简过了。
