# Postman



#### 作者：江伟



#### 邮箱：jiangwei@haomo-studio.com



### Postma简介



Postman是一种网页调试与发送网页http请求的chrome插件，我们可以用来很方便的模拟get或者post或者其他方式的请求来调试接口。

####优点：
* **Team** 可以创建自己团队，团队成员可以共享数据，文件，方便团队之间的合作

* **创建 + 测试** 创建和发送任何的HTTP请求，请求可以保存到历史中再次执行

* **方便管理** 使用Postman Collections为更有效的测试及集成工作流管理和组织APIs

* **文档** 依据你创建的Clollections自动生成API文档,并将其发布成规范的格式



---



### Postman安装



1. 去

 [google商城](https://chrome.google.com/webstore/category/extensions?hl=zh-CN)商城搜索，下载

2. 网页搜索postman，下载



**在request builder中，我们可以通过Postman快速的随意组装出我们希望的request。一般来说，所有的HTTP Request都分成4个部分，URL, method, headers和body。而Postman针对这几部分都有针对性的工具。**



#### URL



URL写的是你的api地址。可以是带有 parameter（参数）,也是按params按钮设置参数。而且路径中有参数的话，params也不显示出来。



![](http://img.blog.csdn.net/20160601171111335)



#### Headers



点击’Headers’按钮，Postman同样会弹出一个键值编辑器。在这里，你可以随意添加你想要的Header attribute，同样Postman为我们通过了很贴心的auto-complete功能，敲入一个字母，你可以从下拉菜单里选择你想要的标准atrribute

![](http://img.blog.csdn.net/20160601172202011)



#### Method



Postman支持所有的Method，而一旦你选择了Method，Postman的request body编辑器会根据的你选择，自动的发生改变

![](http://img.blog.csdn.net/20160601172720544)



#### Request Body



如果我们要创建的request是类似于POST，那我们就需要编辑Request Body，Postman根据body type的不同，提供了4中编辑方式：



* **form-data** 主要用于上传文件

* **x-www-form-urlencoded** 是表单常用的格式

* **raw** 可以用来上传JSON数据

* binary 二进制



![](http://img.blog.csdn.net/20160601173348297)



#### Pre-request Script 和Tests



> Pre-request Script 和Tests两个都是用javascript语言



* Pre-request Script



 定义我们在发送request之前需要运行的一些脚本，应用场景主要是设置全局变量和环境变量。SINPPETS中帮我们写好啦常用的postman



* Tests



 定义发送Request之后，需要用脚本检测的内容，SNIPPETS中帮我们写好啦常用的tests。

 ![](/assets/屏幕快照 2016-12-03 19.23.02.png)



 #### Collections



 ![](/assets/屏幕快照 2016-12-03 20.22.48.png)



* 在数据文件夹的右边的`...`按钮是对文件夹的“增删改”,以及共享,创建副本等。

* 在数据文件夹的右边的`>`按钮，点击后会会看到 `Share`,`Run`,`View Docs`三个按钮。

 * Share 共享功能，可以让Team成员在Team Library查看。

 * Run 打开collection runner界面，collection runner是测试接口的地方

 _**_\*\*_**_ - View Dosc 打开API文档页面





#### Postman Interceptor



功能：



1. 记录浏览器请求并直接导入到Postman，
![](/assets/屏幕快照 2016-12-08 16.08.25.png)
2. 可添加Filter，对浏览器中的请求进行过滤



下载：google商城搜索下载



使用：



1. 开启postman中的卫星标志，接受发送来的‘请求’
 ![](/assets/屏幕快照 2016-12-08 16.08.25.png)
2. 记得卫星开启 postman interceptor 在 off开启拦截filters默认的拦截所有网址。如果要拦截百度的就需要在里面写‘baidu’
 ![](/assets/屏幕快照 2016-12-08 16.19.25.png)
3. 打开history（历史记录）就是能看到拦截的到http请求啦




> 参考文章：

> 1. [http:\/\/www.tuicool.com\/articles\/FJNvQrB](http://www.tuicool.com/articles/FJNvQrB)

> 2. [http:\/\/blog.csdn.net\/u013613428\/article\/details\/51557914](http://blog.csdn.net/u013613428/article/details/51557914)

> 3. [http:\/\/blog.csdn.net\/u013613428\/article\/details\/51557804](http://blog.csdn.net/u013613428/article/details/51557804)

> 4. [http:\/\/www.jianshu.com\/users\/f95d015c62ad\/latest\_articles](http://www.jianshu.com/users/f95d015c62ad/latest_articles)




