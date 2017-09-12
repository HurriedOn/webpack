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
