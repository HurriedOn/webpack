# webpack
essays

## 起步
### 创建目录
首先我们创建一个目录，初始化 npm，以及在本地安装 webpack：
```bash
$ npm install -g webpack
$ mkdir webpackDemo && cd webpackDemo
$ npm init -y
$ npm install --save-dev webpack
```
现在我们将创建以下目录结构和内容：
project：
  * webpackDemo  
    * package.json
    * dist
      * index.html
    * src
      * index.js
           
### 创建一个 bundle 文件

要在 index.js 中打包 lodash 依赖，我们需要在本地安装 library：
```bash
$ npm install --save lodash
```		 
src/index.js：
```js
import _ from 'lodash';
function component() {
  var element = document.createElement('div');
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');
  return element;
}
document.body.appendChild(component());
```						
dist/index.html：
```html
<html>
  <head>
    <title></title>
  </head>
  <body>
    <script src="bundle.js"></script>
  </body>
</html>
```	
				
 执行 webpack，会将我们的脚本作为入口起点，然后输出为 bundle.js：
```bash 
$ webpack src/index.js dist/bundle.js
```	 
## 加载 CSS

为了从 JavaScript 模块中 import 一个 CSS 文件，你需要在 module 配置中 安装并添加style-loader 和 css-loader：
```bash
$ npm install --save-dev style-loader css-loader
```
使用一个配置文件webpack.config.js：
```js
const path = require('path');
module.exports = {
  entry: './src/index.js',
	output: {
	  filename: 'bundle.js',
  },
  module: {
    rules: [
      {test: /\.css$/,use: ['style-loader','css-loader']}
    ]
  }
};
```      
      
现在，让我们再次执行构建，通过使用我们的新配置：
```bash
$ webpack --config webpack.config.js
```     

在项目中添加一个新的 style.css 文件：
* webpackDemo
  * package.json
  * webpack.config.js
  * dist
    * bundle.js
    * index.html
  * src
    * style.css
    * index.js
  * node_modules
				
src/style.css：
```css
.texts {color: red;}
```		
src/index.js：
```js
import _ from 'lodash';
import './style.css';
function component() {
  var element = document.createElement('div');
	// lodash 是由当前 script 脚本 import 导入进来的
	element.innerHTML = _.join(['Hello', 'webpack'], ' ');
	element.classList.add('texts');
	return element;
}
document.body.appendChild(component());
```

package.json ：
```json
{
  "scripts": {
    "build": "webpack --module-bind 'css=style-loader!css-loader'"
  }
}
```

现在运行构建命令：
```bash
$ npm run build
```
