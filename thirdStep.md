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
npm run watch
```
需要刷新浏览器，才能看到修改后的实际效果
