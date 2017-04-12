# 3.7.1 GitBook

#### 作者：江伟

#### 邮箱：jiangwei@haomo-studio.com

### GitBook 简介

GitBook 是一个通过 Git 经和 Markdown 来撰写书籍的工具，最终可以生成 3 种格式：

* 静态站点：包含了交互功能（例如搜索、书签）的站点
* PDF：PDF 格式的文件
* eBook：ePub 格式的电子书文件

####Git
<span class="name">
GitBook 使用 Git 进行写作内容管理。
</span>

- 从用户的角度看，这样能够方便地进行多人协作（连程序源代码都能管好，书籍自然不在话下），还不用学习额外概念或用法

- 从设计实现的角度看，这样能够合理利用已有工具（不重复造轮）满足产品需求，甚至扩展性更好（Git 相关服务能够利用的太多了）

####Markdown
GitBook 使用 Markdown语法书写文章。



### 安装Gitbook

> 安装GitBook最好的办法是通过NPM，所有要先下载node.js

打开“命令提示符”（Mac 系统打开“终端”）输入以下命令安装 GitBook：

```
$ npm install gitbook-cli -g
```

输入以下命令，查看 GitBook 版本的方式检查是否安装成功：

```
$ gitbook -V
```
GitBook可以设置一个样板书:
```
gitbook init
```
``init``命令会成个两个md格式的文件

1. README.md   关于这个本书或者这个章节的介绍
2. SUMMARY.md  目录


####GitBook.com
[GitBook.com](https://www.gitbook.com)是一个易于使用的解决方案编写，出版和主机的书籍。它是用于发布内容和协作上最简单的解决方案。




它具有很好的集成[GitBook编辑器](https://www.gitbook.com/editor)。




>参考网址：

>1. [http://blog.csdn.net/wsyw126/article/details/51733577](http://blog.csdn.net/wsyw126/article/details/51733577)
2. [http://toolchain.gitbook.com/setup.html](http://toolchain.gitbook.com/setup.html)



