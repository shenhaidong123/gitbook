# Yeoman

#### 作者：杨柠瑞

#### 邮箱：yangnr@haomo-studio.com



## 1.简介

Yeoman 是一个通用的脚手架系统,可以迅速的搭建一个新项目,并且能够简化了现有项目的维护。
Yeoman 它自己不能做任何操作,每个操作都是由 generators 基本插件在 Yeoman 环境所完成的。Yeoman有很多公共的 generators并且它很容易的创建一个 generator 去匹配任何工作流。 

工作流包括三种类型的工具：脚手架工具（yo），构建工具（Grunt,gulp）和包管理器（bower,npm）。 

Yeoman的最大优势在于它整合了各种流行的实用工具，提供了一站式的解决方案，使得 Web 应用开发中的很多方面变得简单

— YO：Yeoman核心工具，项目工程依赖目录和文件生成工具，项目生产环境和编译环境生成工具。  
— Grunt：前端构建工具，用于预览和测试项目。  
— Bower：Web 开发的包管理器，概念上类似 npm，npm 专注于 NodeJs 模块，而 bower 专注于 CSS、JavaScript、图像等前端相关内容的管理。

##2.起步
>安装yo和generator(-g为全局安装)

使用nmp安装yo 
	
	npm install -g yo

然后安装需要的 generator（ generator 是一个名字为 generator-*** 的 npm 包。我们可以在Yeoman的官网上找到它们或者使用 "install a generator" 按钮选项来选择，然后去运行yo，去安装这个 webapp generator ）:

	npm install -g generator-webapp
	
用脚手架搭建一个新的项目

	yo webapp


