
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
## 加载 图片
假想，现在我们正在下载 CSS，但是我们的背景和图标这些图片，要如何处理呢？使用 file-loader，我们可以轻松地将这些内容混合到 CSS 中：
```bash
npm install --save-dev file-loader
```
webpack.config.js
```js
  const path = require('path');

  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist')
    },
    module: {
      rules: [
        {
          test: /\.css$/,
          use: [
            'style-loader',
            'css-loader'
          ]
        },
        {
          test: /\.(png|svg|jpg|gif)$/,
          use: [
            'file-loader'
          ]
         }
      ]
    }
  };
```
添加图片到src目录下：
* webpackDemo
  * package.json
  * webpack.config.js
  * dist
    * bundle.js
    * index.html
  * src
    * style.css
    * index.js
    * robot.png
  * node_modules
  
src/index.js:
```js
import _ from 'lodash';
import './style.css';
import Icon from './robot.png';
function component(){
	var element=document.createElement('div');
	// Lodash，现在由此脚本导入
	element.innerHTML=_.join(['hell','webpack'],' ');
	element.classList.add('hello');
	// 将图像添加到我们现有的 div
	var imgIcon=new Image();
	imgIcon.src=Icon;
	element.appendChild(myIcon);
	
	return element;
}
document.body.appendChild(component());

  ```
src/style.css:
```css
.hello{color:red;background:url('./robot.png');}
```
package.json ：
```json
{
  "scripts": {
    "build": "webpack --module-bind 'css=style-loader!css-loader' --module-bind file-loader"
  }
}
```
让我们重新构建，并再次打开 index.html 文件：
```bash
$ npm run build
```
## 加载 字体
file-loader 和 url-loader 可以接收并加载任何文件，然后将其输出到构建目录。这就是说，我们可以将它们用于任何类型的文件，包括字体。让我们更新 webpack.config.js 来处理字体文件：
```js
const path=require('path');
module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist')
    },
    module: {
      rules: [
        {
          test: /\.css$/,
          use: ['style-loader','css-loader']
        },
        {
          test: /\.(png|svg|jpg|gif)$/,
          use: ['file-loader']
         }
      ]
    }
  };
```
在项目中添加一些字体文件：
* webpackDemo
  * package.json
  * webpack.config.js
  * dist
    * bundle.js
    * index.html
  * src
    * style.css
    * index.js
    * robot.png
    * FZLTQianXi.ttf
  * node_modules
  
src/style.css:
 ```css
@font-face {
font-family:'webfont';
  src: url('./FZLTQianXi.ttf') format('ttf');
  font-weight:400;
  font-style: normal;
}
.hello{color:red;background:#6bb0da;font-family:'webfont';}
```
让我们重新构建，并再次打开 index.html 文件：
```bash
$ npm run build
```
