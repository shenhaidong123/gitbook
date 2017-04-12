# Swagger接口定义

#### 作者：李兴朋

#### 邮箱：lixingpeng@haomo-studio.com

## 1.Swagger

### **1.1.Swagger** 简介及发展史

[Swagger](http://developers.helloreverb.com/swagger/)：是一个规范和完整的框架，可以用于生成、描述、调用和可视化 RESTful 风格的 Web 服务，总体目标是使客户端和文档系统与服务器以同样的速度进行更新。Swagger倾向于在线测试接口和数据。并且这是一个完全开源的项目，并且它也是一个基于Angular的成功案例，我们可以下载源码并自己部署它，也可以修改它或集成到我们自己的软件中。

**发展史：**

Swagger的创始人： [Tony Tam](https://www.linkedin.com/in/tonytam)

2009年Swagger框架启动，但是在当时Swagger API框架只是在Reverb内部开发的一个项目 ；

2011-10-11   1.0.1版本；2013-03-08    1.0.13；

2014年5月  Swagger的开放[工作小组](https://github.com/swagger-api/swagger-spec) 成立；

Swagger 2.0.24在2014年9月12日正式发布；

Swagger规范未来的发展将由[SmartBear](http://smartbear.com/about-us/)负责，这是一家位于马萨诸塞州的软件工具公司。

2016-07-20  2.1.5版本；2016-10-14  2.2.6版本；

### 1.2.Swagger安装使用

**Swagger Editor安装：**

需要下载一个Swagger Editor，也可以直接使用在线版本[http://editor.swagger.io/](http://editor.swagger.io/)。

现在地址：`git clone https://githup.com/swagger-api/swagger-editor.git`

下载完成后进行本地配置（前提是已经安装好了Node.js），命令如下：

1.`cd swagger-editor`

2.npm install -g http-server

3.http-server （默认的是：8080端口）

4.http-server -p 7000 可以修改端口（临时）

![](/assets/WechatIMG15.jpeg)

![](/assets/屏幕快照 2017-01-11 下午10.47.05.png)

![](/assets/WechatIMG16.jpeg)

![](/assets/WechatIMG17.jpeg)

**Swagger-UI的安装：**

现在地址：`git clone https://githup.com/swagger-api/swagger-ui.git`

**Swagger UI的本地配置：**

1,**创建一个 node目录**

_mkdir node_

2.**初始化 node，创建package.json**

_~ &gt; cd node_

_~ &gt; node &gt;npm init_

![](/assets/WechatIMG18.jpeg)

3**安装express**

_~ &gt; node &gt;npm install express --save_

4.**创建index.js**

_~ &gt; node &gt; vim index.js_

**将一下代码复制到index.js中：**  
_var express = require\('express'\);_

_var app = express\(\);_

_app.use\('/static', express.static\('public'\)\);_

_app.get\('/', function \(req, res\) {_

_res.send\('Hello World!'\);_

_}\);_

_app.listen\(5000, function \(\) {_

_console.log\('Example app listening on port 5000!'\);_

_}\);_

5**.创建public文件夹**：

~ &gt; node &gt; mkdir public

**把Swagger-UI文件中dist目录下的文件全部复制到public中**：

![](/assets/WechatIMG19.jpeg)

**开启node**

~ &gt; node &gt; node  index.js

打**开浏览器输入：localhost:3000/static/index.html**

### 1 .3.**Swagger Editor界面功能的介绍**

将安装好打开后界面如下：

```
当我们修改了API的定义之后，在编辑器右侧就可以看到相应的API文档了，而且永远是最新的。
```

![](/assets/202F.tmp.jpg)

它还能够帮助我们自动生成不同语言的客户端的代码。Swagger是基于插件来实现各种不同的语言的，所以，如果已经提供的语言中没有你正在用的，你也可以自己实现相应的插件，甚至是从源代码级别进行定制化。

![](/assets/DCDB.tmp.jpg)

### 1.4.Swagger参数详解

![](/assets/CE6D.tmp.jpg)

`get  是指这个接口对应的请求方式`

`tag   操作的标签`

`summary  是对这个借口进行的一个短描述`

这三项显示在接口的顶部，图示：

![](/assets/47D.tmp.jpg)

`description：对接口的描述`

`parameters:   接口需要传递的参数`

`- name: X-Auth-Token 参数的名字`

`in: header 他的值:header、path、formData Query body`

`description: /auth接口获取的token`

`required: true 两个值:true、false；true为必填项，false为非必填项`

`type: string 参数格式`

![](/assets/25F5.tmp.jpg)

`responses:   表示请求的返回值`

`200  表示请求成功的状态`

`description: 用户详情    在请求成功时对返回值的描述`

`schema:`

`$ref: '#/definitions/StUser'    请求成功后的返回值`

`default：表示请求失败`

`description: 未知错误      在请求失败时对返回值的描述`

`schema:`

`$ref: '#/definitions/Error'   请求失败后的返回值`

![](/assets/72FE.tmp.jpg)

## 2.与Swagger类似的技术

[API Blueprint](http://apiblueprint.org/)：提供跨越API整个周期的工具，这样可以和别人讨论你的API，可以产生自动文档或一个测试案例。它是使用Markdown来定义API的，Markdown相比RAML、JSON门槛又降低了一大截。同时API Blueprint与Swagger、RAML一样也能提供可视化的文档界面与Mock Server的功能。不过其Mock能力比较弱。

[RAML](http://raml.org/index.html)：是一种[**RESTful**](http://www.jdon.com/rest.html) API 建模语言\(RESTful API Modeling Language :RAML\)， 是对 RESTful API的一种简单和直接的描述 ， 它鼓励重用 激活发现和模式分享，定位在最佳实践的最优实现。它是一种让人们易于阅读并且能让机器对特定的文档能解析的语言。RAML 是基于 YAML,能帮助设计 RESTful API 和鼓励对API的发掘和重用,依靠标准和最佳实践从而编写更高质量的API。通过RAML定义,因为机器能够看得懂,所以可以衍生出一些附加的功能服务,像是解析并自动生成对应的客户端调用代码、服务端代码 结构, API说明文档。

## _3._参考资料及相关内容阅读

知乎网比较详细的Swagger介绍：[https://zhuanlan.zhihu.com/p/21353795](https://zhuanlan.zhihu.com/p/21353795)

RESTful架构风格介绍：[http://www.cnblogs.com/chinajava/p/5871305.html](http://www.cnblogs.com/chinajava/p/5871305.html)

[http://www.cnblogs.com/whitewolf/p/4686154.html](http://www.cnblogs.com/whitewolf/p/4686154.html)

swagger ；[https://topbitdu.gitbooks.io/swagger-document-convention/content/](https://topbitdu.gitbooks.io/swagger-document-convention/content/)

Swagger-UI 的官方地址：[http://swagger.wordnik.com](http://swagger.wordnik.com/)

Github上的项目地址：[https://github.com/wordnik/swagger-ui](https://github.com/wordnik/swagger-ui)

官方提供的demo地址：[http://petstore.swagger.wordnik.com/](http://petstore.swagger.wordnik.com/)

官方：[https://github.com/adrianbk/swagger-springmvc-demo](https://github.com/adrianbk/swagger-springmvc-demo)

Swagger的发展史：[http://www.infoq.com/cn/articles/swagger-interview-tony-tam](http://www.infoq.com/cn/articles/swagger-interview-tony-tam)

Swagger本地设置：[http://www.jianshu.com/p/d6626e6bd72c](http://www.jianshu.com/p/d6626e6bd72c)

