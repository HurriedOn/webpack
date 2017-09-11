## 选择一个开发工具
webpack 中有几个不同的选项，可以帮助你在代码发生变化后自动编译代码：
+ webpack's Watch Mode
+ webpack-dev-server
+ webpack-dev-middleware
### 使用观察模式
package.json:
```json
"scripts": {
    "build": "webpack --module-bind 'css=style-loader!css-loader' --module-bind file-loader",
    "test": "echo \"Error: no test specified\" && exit 1",
    "watch": "webpack --watch"
  }
```  
src/print.js:
```js
export default function printMe(){
	console.log("I get called from print.js");
}
```
命令行中运行：
```bash
$ npm run watch
```
需要刷新浏览器，才能看到修改后的实际效果。如果要退出监视模式，Ctrl + C是一个可供的选择。
### 使用 webpack-dev-server
为你提供了一个简单的 web 服务器，并且能够实时重新加载：
```bash
$ npm install --save-dev webpack-dev-server
```
修改配置文件，告诉开发服务器(dev server)，在哪里查找文件，webpack.config.js：
```js
const path=require('path');
const HtmlWebpackPlugin=require('html-webpack-plugin');
const CleanWebpackPlugin=require('clean-webpack-plugin');
module.exports={
	entry:{
		app:'./src/index.js',
		print:'./src/print.js'
	},
	devServer:{
		contentBase:'./dist'
	},
	plugins:[
		new CleanWebpackPlugin(['dist']),
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
package.json:
```json
"scripts": {
    "build": "webpack --module-bind 'css=style-loader!css-loader' --module-bind file-loader",
    "test": "echo \"Error: no test specified\" && exit 1",
    "watch": "webpack --watch",
    "start": "webpack-dev-server --open"
  }
```
直接运行命令:
```bash
npm start
```
在 localhost:8080 下建立服务，将 dist 目录下的文件，作为可访问文件。

