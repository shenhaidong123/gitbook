# NPM培训

## 第一章 NPM介绍

### 1.1 npm历史 

2009年，[npm（Node 包管理器）初次发布早期预览版；](https://groups.google.com/forum/?hl=en#!topic/nodejs/erDWyS4xPw8)

2011年，[npm 1.0：发布；](https://nodejs.org/en/blog/npm/npm-1-0-released/)

2015年，[npm 支持私有模块](https://www.npmjs.com/private-modules) 。
### 1.2 相关工具

**bower:** Bower是一个客户端技术的软件包管理器，它可用于搜索、安装和卸载如JavaScript、HTML、CSS之类的网络资源

**yarn:** facebook公司推出的一款快速、可靠、安全的依赖管理工具

### 1.3 未来展望

npm作为随同nodeJS一起安装的包管理工具，在node包管理领域具有天然的优势，是目前javascript工作者使用最广的js包管理工具；

2016年10月，facebook公司推出一款新的包管理工具yarn，相对于npm具有更快，更可靠，更安全的优势，对npm使用率带来巨大的冲击，但是目前yarn还有一些不完善的地方，比如不能够独立升级某个依赖等，所以在一定时间内npm仍将是使用最广的JavaScript包管理器。

## 第二章 npm安装和使用

### 2.1 npm的安装

由于新版的nodejs已经集成了npm，安装nodeJS就安装好npm。

[下载安装 nodeJS 和 npm](https://nodejs.org/en/) 

可通过 **"npm -v" **来测试是否成功安装。命令如下：

`$ npm -v  (可查看npm的安装版本)`

如果安装成功显示：

![](/assets/npmversion.png)

### 2.2 npm的使用

npm 的包安装分为本地安装（local）、全局安装（global）两种，从敲的命令行来看，差别只是有没有-g而已，如下：

```
npm   install  <package-name>             #本地安装

npm   install -g <package-name>       #全局安装
```

下图是在全局环境下安装webpack实例：

![](/assets/npminstallwebpack.png)

在项目工程文件下安装依赖包并保存需要使用先创建package.json文件，下面的代码和图片表示在 ~/DeskTop/webDemo 文件中创建 package.json：

```
cd ~/DeskTop/webDemo
npm -init
```

![](/assets/npminit.png)

输入npm init 后，根据指示直接按下enter键，即可生成package.json，package.json文件内容如下:

```
{
  "name": "npm",
  "version": "1.0.0",
  "description": "npm",
  "main": "npm",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "npm"
  ],
  "author": "shd",
  "license": "MIT"
}
```
install命令可以使用不同参数，指定所安装的模块属于哪一种性质的依赖关系，即出现在packages.json文件的哪一项中。
- –-save：模块名将被添加到dependencies，可以简化为参数-S。
- –-save-dev: 模块名将被添加到devDependencies，可以简化为参数-D。

```
npm i lodash --save  # --save用于将安装包名称添加到package.json中
```
![](/assets/npminstalllodash.png)

![](/assets/packagelodash.png)

安装完毕后会在工作目录下产生一个node\_modules目录，其目录下就是安装的各个node模块;

参数使用 --save 表示将在项目文件下将在package.json的项目依赖选项中添加了下载的node模块；
在其他地方使用该项目时，使用npm install即可自动下载在package.json选项中存在的依赖包，而不需要输入module name

更新本地/全局包

```
npm update <package-name>

npm update -g <package-name>
```

安装特定版本号的包：

```
npm install <package-name>@<version>
```
实例如下图：

![](/assets/sureversion.png)

nodejs集成了npm，因此无法全局升级npm，需要在nodejs的安装目录下局部升级npm。例：

`cd "e:\nodejs"   进入node.js的安装目录`

`npm update npm    更新NPM`


### 2.3 使用淘宝 NPM 镜像

在国内直接使用 npm 的官方镜像是非常慢的，推荐使用淘宝 NPM 镜像。

淘宝 NPM 镜像是一个完整 npmjs.org 镜像，你可以用此代替官方版本\(只读\)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

你可以使用淘宝定制的 cnpm \(gzip 压缩支持\) 命令行工具代替默认的 npm：

`$ npm install -g cnpm --registry=https://registry.npm.taobao.org`

### 2.4 其他命令

```
$ npm uninstall express #卸载模块
$ npm update express    #更新模块
$ npm search express    #搜索模块
$ npm info express      #查看模块信息
$ npm ls                #查看已经安装的模块列表

```

## 参考文档：

NPM使用介绍-菜鸟教程： [http://www.runoob.com/nodejs/nodejs-npm.html](http://www.runoob.com/nodejs/nodejs-npm.html)

Node.js&NPM的安装与配置：[http://www.infoq.com/cn/articles/nodejs-npm-install-config](http://www.infoq.com/cn/articles/nodejs-npm-install-config)

node.js的配置以及向git提交代码：[http://www.tuicool.com/articles/aiaQR3b](http://www.tuicool.com/articles/aiaQR3b)

知乎上关于node.js 的话题：[https://www.zhihu.com/topic/19569535/top-answers](https://www.zhihu.com/topic/19569535/top-answers)

知乎中关于npm的话题：[https://www.zhihu.com/question/24414899](https://www.zhihu.com/question/24414899)

npm，bower，jamjs等包管理工具各自的特性及区别：[https://www.zhihu.com/question/24414899](https://www.zhihu.com/question/24414899)

