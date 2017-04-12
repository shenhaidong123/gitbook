# webpack

#### 作者：申海东

#### 邮箱：shenhaidong@haomo-studio.com

参考网址： [http://webpack.github.io/docs/](http://webpack.github.io/docs/)

#### 简介：

Webpack 是当下最热门的前端资源模块化管理和打包工具。他的作用：
- 打包工具，可以将js文件打包为一个js文件，并且加载加载他们的依赖；
- webpack默认支持commonjs规范；
- 对于webpack一切都是模块化的。是一个模块加载器（js，css，img等等）。
- webpack默认只支持js文件打包。打包其他工具需要使用loader来编译其他文件；比如json，css等文件

####webpack的使用

#### webpack安装

- webpack依赖于nodejs，使用npm下载；

  - 全局安装：
    ```
    $ npm install webpack
    ```
  - 安装到项目目录下：
  ```
    $ npm install webpack --save-dev
  ```

#### webpack基本入门：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;webpack使用的两种形式：终端命令行和通过配置文件来使用webpack；

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;下面我们通过实例来说明webpack的使用方法并说明优缺点。

- **入门实例**：

  1. 准备阶段：

    - 创建一个空的文件夹作为我们的联系项目文件夹，使用 **npm init** 命令初始化一个package.json文件：
    ```
     npm install
    ```
    - package.json文件准备就绪，我们可以在项目中安装webpack：
    ```
      npm install --save-dev webpack //本项目安装；
      npm install webpack -g         //全局安装      
    ```

     - 在之前创建的文件夹下创建子文件：js文件夹和app文件：目录结构如下图所示

    ![](/assets/屏幕快照 2017-01-09 下午10.06.41.png)

     - index.html文件内容：

     ```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="root"></div>
<script src="bundle.js"></script>
</body>
</html> 
     ```
     - main.js文件内容为：

     ```
var greeter = require('./greeter');
var root = document.getElementById('root');
root.appendChild(greeter());
     ```

     - greeter.js文件内容为：

     ```
     module.exports = function(){
    var content = document.createElement('div');
    content.textContent = 'hallow everyone';
    return content;
};
     ```

  2. webpack简单使用：

    - webpack全局安装环境下命令行使用：

   ```
   webpack {入口文件位置} {bundle.js文件位置}
   ``` 

    - webpack项目文件夹中安装：
    ```
    node_modules/.bin/webpack {入口文件位置} {bundle.js文件位置}
    ```
    结果如下显示：

    ![](/assets/屏幕快照 2017-01-09 下午10.23.09.png)

   注： 只需要指定一个入口文件，webpack将自动识别项目所依赖的其它文件，

    - 通过配置文件来使用webpack：创建webpack.config.js,内容如下：
    ```
     module.exports = {
     entry: __dirname + "/js/main.js",  //入口文件
     output: {
        path: __dirname + "/app",       //打包后文件存放位置
         filename: "bundle.js"          //打包后输出文件名字
     }
 };
    ```
    配置好文件之后我们使用命令webpack（非全局安装使用：node_modules/.bin/webpack）
 注： 在任何模块文件内部，可以使用__dirname变量获取当前模块文件所在目录的完整绝对路径。

       **扩展：**我们可以npm来引导webpack执行：配置package.json文件如下图，运行npm start即可执行webpack;
    ```
    {
  "name": "hd",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^1.14.0"
  }
}
    ```

    运行命令如下图所示:

    ![](/assets/屏幕快照 2017-01-09 下午10.42.04.png)

####使用webpack构建本地服务器：

     ```
     npm install --save-dev webapck-dev-server
     ```
   - devserver作为webpack配置项中的一项，具有以下配置选项：

   | 配置选项       | 	功能描述          | 
   | -------------- |:------------------:| 
   | contentBase    | 默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录（本例设置到“public"目录） | 
   | port    | 设置默认监听端口，如果省略，默认为”8080“     | 
   | inline | 设置为true，当源文件改变时会自动刷新页面     |  
   | colors | 设置为true，使终端输出的文件为彩色的      | 
   | historyApiFallback | 在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为true，所有的跳转将指向index.html | 

   继续把这些命令加到webpack的配置文件中，现在的配置文件如下所示:

   ```
   module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },

  devServer: {
    contentBase: "./public",//本地服务器所加载的页面所在的目录
    colors: true,//终端中输出结果为彩色
    historyApiFallback: true,//不跳转
    inline: true//实时刷新
  } 
}
   ```

#### loaders


  1. **作用：** 通过使用不同的loader，webpack通过调用外部的脚本或工具可以对各种各样的格式的文件进行处理，比如:

   - 分析JSON文件并把它转换为JavaScript文件
   - 把下一代的JS文件（ES6，ES7)转换为现代浏览器可以识别的JS文件
   - 对React的开发而言，合适的Loaders可以把React的JSX文件转换为JS文件;

  2. **配置：** Loaders需要单独安装并且需要在webpack.config.js下的modules关键字下进行配置,Loaders的配置选项包括以下几方面

   - `test` 一个匹配loaders所处理的文件的拓展名的正则表达式（必须）；
   - `loader` loader的名称（必须）;
   - `include/exclude` 动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
   - `query` 为loaders提供额外的设置选项（可选）；

  3. **简单json实例：**
   继续上面的例子，我们把greeter.js里的问候消息放在一个单独的JSON文件里,并通过合适的配置使greeter.js可以读取该JSON文件的值，配置方法如下
   ```
npm install --save-dev json-loader
   ```
   - 配置loader
     
   ```
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },

  module: {//在配置文件里添加JSON loader
    loaders: [
      {
        test: /\.json$/,
        loader: "json"
      }
    ]
  },

  devServer: {
    contentBase: "./public",
    colors: true,
    historyApiFallback: true,
    inline: true
  }
}

   ```

    - 创建带有问候信息的JSON文件(命名为config.json)

    ```
//config.json
{
  "greetText": "Hi there and greetings from JSON!"
}
   ```
    - 更新greeter.js

    ```
module.exports = function() {
  var greet = document.createElement('div');
  greet.textContent = config.greetText;
  return greet;
};
   ```

   运行npm start即可；

  4. **babel实例：**
   - 下载所需依赖

   ```
npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react
   ```   
   - 配置webpack.config.js

   ```
   module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },

  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: "json"
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel',//在webpack的module部分的loaders里进行配置即可
        query: {
          presets: ['es2015','react']
        }
      }
    ]
  },

  devServer: {
    contentBase: "./public",
    colors: true,
    historyApiFallback: true,
    inline: true
  }
}
   ```   
 现在webpack的配置已经允许使用ES6以及JSX的语法;我们继续上面的例子，使用react和es语法；需要先下载react 和react-dom：
```
npm install --save react react-dom
```

 - greeter.js 文件内容：

  使用es6语法和JSX语法来编写greeter.js

  ```
npm install --save react react-dom
//Greeter,js
import React, {Component} from 'react'
import config from './config.json';
class Greeter extends Component{
  render() {
    return (
      <div>
        {config.greetText}
      </div>
    );
  }
}
export default Greeter
  ```
   - 在main.js中使用ES6的模块定义和渲染Greeter模块：
```
import React from 'react';
import {render} from 'react-dom';
import Greeter from './Greeter';
render(<Greeter />, document.getElementById('root'));
```
   - 在webpack.config.js中配置babel：

   Babel其实可以完全在webpack.config.js中进行配置，但是考虑到babel具有非常多的配置选项，在单一的webpack.config.js文件中进行配置往往使得这个文件显得太复杂，因此一些开发者支持把babel的配置选项放在一个单独的名为 ".babelrc" 的配置文件中。我们现在的babel的配置并不算复杂，不过之后我们会再加一些东西，因此现在我们就提取出相关部分，分两个配置文件进行配置（webpack会自动调用.babelrc里的babel配置选项），如下：
```
// webpack.config.js
module.exports = {
  devtool: 'eval-source-map',
  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },
  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: "json"
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel'
      }
    ]
  },
  devServer: {...} // Omitted for brevity
}
```
```
//.babelrc
{
  "presets": ["react", "es2015"]
}
```
  - **一切皆模块**

   Webpack有一个不可不说的优点，它把所有的文件都可以当做模块处理，包括你的JavaScript代码，也包括CSS和fonts以及图片等等等，只有通过合适的loaders，它们都可以被当做模块被处理。

   -  **CSS**

   webpack提供两个工具处理样式表，css-loader 和 style-loader，二者处理的任务不同，css-loader使你能够使用类似@import 和 url(...)的方法实现 require()的功能,style-loader将所有的计算后的样式加入页面中，二者组合在一起使你能够把样式表嵌入webpack打包后的JS文件中。
  继续上面的例子:

   ```
//安装
npm install --save-dev style-loader css-loader
   ```
```
//使用
module.exports = {
  devtool: 'eval-source-map',
  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/build",
    filename: "bundle.js"
  },
  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: "json"
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel'
      },
      {
        test: /\.css$/,
        loader: 'style!css'//添加对样式表的处理
      }                    // !的作用在于使同一个文件使用不同的loader
    ]
  }
}
```

  接下来在js文件里创建一个“main.css”文件，设置样式属性：
```
html {
  box-sizing: border-box;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
}
*, *:before, *:after {
  box-sizing: inherit;
}
body {
  margin: 0;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
}
h1, h2, h3, h4, h5, h6, p, ul {
  margin: 0;
  padding: 0;
}
```
webpack只有单一的入口，其它的模块需要通过 import, require, url等导入相关位置，为了让webpack能找到”main.css“文件，我们把它导入”main.js “中，如下:
```
//main.js
import React from 'react';
import {render} from 'react-dom';
import Greeter from './Greeter';
import './main.css';//使用require导入css文件
render(<Greeter />, document.getElementById('root'));
```

  **注：**
   通常情况下，css会和js打包到同一个文件中，并不会打包为一个单独的css文件，不过通过合适的配置webpack也可以把css打包为单独的文件的。
不过这也只是webpack把css当做模块而已，咱们继续看看一个真的CSS模块的实践。

  - css预处理器：

  Sass 和 Less之类的预处理器是对原生CSS的拓展，它们允许你使用类似于variables, nesting, mixins, inheritance等不存在于CSS中的特性来写CSS，CSS预处理器可以这些特殊类型的语句转化为浏览器可识别的CSS语句，在webpack里使用相关loaders进行配置就可以使用了，以下是常用的CSS 处理loaders：

   - Less Loader
   - Sass Loader
   - Stylus Loader
```
npm install --save-dev postcss-loader autoprefixer
```
接下来，在webpack配置文件中进行设置，只需要新建一个postcss关键字，并在里面申明依赖的插件，如下，现在css会自动根据Can i use里的数据添加不同前缀了。
```
//webpack配置文件
module.exports = {
  devtool: 'eval-source-map',
  entry: __dirname + "/app/main.js",
  output: {...},
  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: "json"
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel'
      },
      {
        test: /\.css$/,
        loader: 'style!css?modules!postcss'
      }
    ]
  },
  postcss: [
    require('autoprefixer')//调用autoprefixer插件
  ],
  devServer: {...}
}
```

####插件（Plugins）

插件（Plugins）是用来拓展Webpack功能的，它们会在整个构建过程中生效，执行相关的任务。
Loaders和Plugins常常被弄混，但是他们其实是完全不同的东西，可以这么来说，loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个，插件并不直接操作单个文件，它直接对整个构建过程其作用。

#####插件的使用
要使用某个插件，我们需要通过npm安装它，然后要做的就是在webpack配置中的plugins关键字部分添加该插件的一个实例（plugins是一个数组）继续看例子，我们添加了一个实现版权声明的插件。
```
//webpack.config.js
var webpack = require('webpack');

module.exports = {
  devtool: 'eval-source-map',
  entry:  __dirname + "/app/main.js",
  output: {...},

  module: {
    loaders: [
      { test: /\.json$/, loader: "json" },
      { test: /\.js$/, exclude: /node_modules/, loader: 'babel' },
      { test: /\.css$/, loader: 'style!css?modules!postcss' }//这里添加PostCSS
    ]
  },
  postcss: [
    require('autoprefixer')
  ],

  plugins: [
    new webpack.BannerPlugin("Copyright Flying Unicorns inc.")//在这个数组中new一个就可以了
  ],

  devServer: {...}
}
```
#####常用的插件：
1. **HtmlWebpackPlugin**
<hr />
     这个插件的作用是依据一个简单的模板，生成最终的HTML5文件，这个文件中自动引用了你打包后的JS文件。每次编<译都在文件名中插入一个不同的哈希值。
   - 安装
```
npm install --save-dev html-webpack-plugin
```
这个插件自动完成了我们之前手动做的一些事情，在正式使用之前需要对一直以来的项目结构做一些改变：
  - 移除public文件夹，利用此插件，HTML5文件会自动生成，此外CSS已经通过前面的操作打包到JS中了，public文件夹里。
  - 在app目录下，创建一个Html文件模板，这个模板包含title等其它你需要的元素，在编译过程中，本插件会依据此模板生成最终的html页面，会自动添加所依赖的 css, js，favicon等文件，在本例中命名模板文件名称为index.tmpl.html，模板源代码如
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Webpack Sample Project</title>
  </head>
  <body>
    <div id='root'>
    </div>
  </body>
</html>
```
  - 更新webpack的配置文件，方法同上,新建一个build文件夹用来存放最终的输出文件:
```
  var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
  devtool: 'eval-source-map',
  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/build",
    filename: "bundle.js"
  },
  module: {
    loaders: [
      { test: /\.json$/, loader: "json" },
      { test: /\.js$/, exclude: /node_modules/, loader: 'babel' },
      { test: /\.css$/, loader: 'style!css?modules!postcss' }
    ]
  },
  postcss: [
    require('autoprefixer')
  ],
  plugins: [
    new HtmlWebpackPlugin({
      template: __dirname + "/app/index.tmpl.html"//new 一个这个插件的实例，并传入相关的参数
    })
  ],
  devServer: {
    colors: true,
    historyApiFallback: true,
    inline: true
  }
}
```
2. **Hot Module Replacement**
<hr />
Hot Module Replacement（HMR）也是webpack里很有用的一个插件，它允许你在修改组件代码后，自动刷新实时预览修改后的效果,需要做两项配置:
 - 在webpack配置文件中添加HMR插件；
 - 在Webpack Dev Server中添加“hot”参数
不过配置完这些后，JS模块其实还是不能自动热加载的，还需要在你的JS模块中执行一个Webpack提供的API才能实现热加载，虽然这个API不难使用，但是如果是React模块，使用我们已经熟悉的Babel可以更方便的实现功能热加载。
具体实现方法如下:
        - Babel和webpack是独立的工具
        - 二者可以一起工作
        -  二者都可以通过插件拓展功能
        - HMR是一个webpack插件，它让你能浏览器中实时观察模块修改后的效果，但 是如果你想让它工作，需要对模块进行额外的配额；
        - Babel有一个叫做react-transform-hrm的插件，可以在不对React模块进行额外的配置的前提下让HMR正常工作；
   实例：
```
//webpack中的配置
var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
  devtool: 'eval-source-map',
  entry: __dirname + "/app/main.js",
  output: {
    path: __dirname + "/build",
    filename: "bundle.js"
  },
  module: {
    loaders: [
      { test: /\.json$/, loader: "json" },
      { test: /\.js$/, exclude: /node_modules/, loader: 'babel' },
      { test: /\.css$/, loader: 'style!css?modules!postcss' }
    ]
  },
  postcss: [
    require('autoprefixer')
  ],
  plugins: [
    new HtmlWebpackPlugin({
      template: __dirname + "/app/index.tmpl.html"
    }),
    new webpack.HotModuleReplacementPlugin()//热加载插件
  ],
  devServer: {
    colors: true,
    historyApiFallback: true,
    inline: true,
    hot: true
  }
}
```
安装react-transform-hmr:
```
npm install --save-dev babel-plugin-react-transform react-transform-hmr
```
配置Babel
```
{
  "presets": ["react", "es2015"],
  "env": {
    "development": {
    "plugins": [["react-transform", {
       "transforms": [{
         "transform": "react-transform-hmr",

         "imports": ["react"],

         "locals": ["module"]
       }]
     }]]
    }
  }
}
```
**resolve配置**
可以省略的扩展名；
```
    resolve:{
        "extensions": ["",".js", ".css", ".json"]
    }
```
![](/assets/屏幕快照 2017-01-13 下午9.28.36.png)
 


####打包工具
类似的打包工具有：gulp，browserfy，webpack等等。

####参考网站：
- webpack：http://webpack.github.io/docs

- webpack中文网站：https://www.w3ctech.com/topic/1557

- webpack在react中的应用：http://www.infoq.com/cn/articles/react-and-webpack

后面两个网站的内容非常适合入门级webpack人员使用。

####补充信息
Source map：一个信息文件，里面储存着位置信息。也就是说，转换后的代码的每一个位置，所对应的转换前的位置。

参考网址：http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html






 
