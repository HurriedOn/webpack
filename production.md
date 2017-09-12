## 配置
开发环境(development)和生产环境(production)，通常为每个环境编写彼此独立的 webpack 配置。将这些配置合并在一起，我们将使用一个名为 webpack-merge 的工具：
```bash
$ npm install --save-dev webpack-merge
```
项目结构:
* webpackDemo
    * package.json
    * webpack.common.js
    * webpack.dev.js
    * webpack.prod.js
    * dist
    * src
        * index.js
        * math.js
    * node_modules

webpack.common.js：
```js
const path=require('path');
const CleanWebpackPlugin=require('clean-webpack-plugin');
const HtmlWebpackPlugin=require('html-webpack-plugin');

module.exports={
	entry:{
		app:'./src/index.js'
	},
	plugins:[
		new CleanWebpackPlugin(['dist']),
		new HtmlWebpackPlugin({title:'production'})
	],
	output:{
		filename:'[name].bundle.js',
		path:path.resolve(__dirname,'dist')
	}
	
}
```
webpack.dev.js：
```js
const merge=require('webpack-merge');
const common=require('./webpack.common.js');

module.exports=merge(common,{
	devtool:'inline-source-map',
	devServer:{contentBase:'./dist'}
})
```
webpack.prod.js：
```js
const merge=require('webpack-merge');
const UglifyJSPlugin=require('uglifyjs-webpack-plugin');
const common=require('./webpack.common.js');

module.exports=merge(common,{
	plugins:[new UglifyJSPlugin()]
})
```

## NPM Scripts
把 scripts 重新指向到新配置，package.json：
```json
"scripts": {
    "start": "webpack-dev-server --open --config webpack.dev.js",
    "build": "webpack --config webpack.prod.js"
  }
```
## source map
webpack.prod.js：
```js
  const merge = require('webpack-merge');
  const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
  const common = require('./webpack.common.js');

  module.exports = merge(common, {
    devtool: 'source-map',
    plugins: [
     new UglifyJSPlugin({
       sourceMap: true
     })
    ]
  })
  ```
## 指定环境
webpack.prod.js：
```js
 const webpack = require('webpack');
  const merge = require('webpack-merge');
  const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
  const common = require('./webpack.common.js');

  module.exports = merge(common, {
    devtool: 'cheap-module-source-map',
    plugins: [
     new UglifyJSPlugin()
     new UglifyJSPlugin(),
     new webpack.DefinePlugin({
       'process.env': {
         'NODE_ENV': JSON.stringify('production')
       }
     })
    ]
  })
  ```
src/index.js：
```js
import { cube } from './math.js';

if (process.env.NODE_ENV !== 'production') {
   console.log('Looks like we are in development mode!');
}

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
