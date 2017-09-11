## 准备
项目目录：
* webpackDeom
    * package.json
    * dist
        * index.html
        * bundle.js
    * src
        * index.js
        * print.js
    * node_modules.js
    
src/print.js:
```js
export default function printMe(){
	console.log("来至于prints.js");
}
```
src/index.js:
```js
import _ from 'lodash';
import printMe from './print.js';
function component(){
	var element=document.createElement('div');
	// Lodash，现在由此脚本导入
	element.innerHTML=_.join(['hell','webpack'],' ');
	
	var btn=document.createElement('button');
	btn.innerHTML='点击后控制台输出';
	btn.onclick=printMe;
	element.appendChild(btn);
	
	return element;
}
document.body.appendChild(component());
```
dist/index.html:
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script src="./print.bundle.js"></script>
	</head>
	<body>
		<script src="./app.bundle.js"></script>
	</body>
</html>
```
webpack.config.js:
```js
const path=require('path');
module.exports={
	entry:{
		app:'./src/index.js',
		print:'./src/print.js'
	},
	output:{
		filename:'[name].bundle.js',
		path: path.resolve(__dirname, 'dist')
	}
};
```
现在，让我们再次执行构建，通过使用我们的新配置：
```bash
$ webpack --config webpack.config.js
```
## 设定 HtmlWebpackPlugin
首先安装插件
```bash
$  npm install --save-dev html-webpack-plugin
```
HtmlWebpackPlugin 会默认生成 index.html 文件。这就是说，它会用新生成的 index.html 文件，把我们的原来的替换。
调整 webpack.config.js 文件：
```js
const path=require('path');
const HtmlWebpackPlugin=require('html-webpack-plugin');
module.exports={
	entry:{
		app:'./src/index.js',
		print:'./src/print.js'
	},
	plugins:[
		new HtmlWebpackPlugin({
			title:'Output Management'
		})
	],
	output:{
		filename:'[name].bundle.js',
		path: path.resolve(__dirname, 'dist')
	}
};
```
再次执行构建，通过使用我们的新配置：
```bash
$ webpack --config webpack.config.js
```
