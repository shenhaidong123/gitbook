# 4.3.1 RestAngular应用

#### 作者：刘冉冉

#### 邮箱：liuranran@haomo-studio.com

参考文档   https://github.com/mgonto/restangular
Angularjs获取服务端口数据的方法有三种，Restangular是其中一种，还有$http和$resouce。他们都都是angualr内置的服务。

      其中$http和Restangular返回promise。
        promise是一种以函数作为then的属性值的对象。
        then(function(data),function(error),function(progress))
        then里面有三个参数，分别代表promise被装载数据的时候调用，promise失败时调用，和在promess事件被触发的时候调用。前两者常用，第三个只有参数被传入的时候才调用，可以被忽略掉。
        then方法在两个function回调完成之后，返回一个新的promise，这样就可以链式调用。回调的返回值就是一个promise。如果回调异常，promise就会把状态变成设为失败。
- ##$http
$http方法只是简单的封装了浏览器原生的XMLHttpRequest对象。
$http只能接受一个参数的函数，这个参数是一个对象，对象包含了用来生成http 请求的配置内容.
 
      var promise=$http({
       method:'GET',
       url:'data.json'
       })
     由于$http返回一个promise对象，包含success和error两个方法。可以在响应返回时用then方法来处理回调。如果响应状态码为200和299之间，认为响应成功。success回调会被调用，否则调用error
  - ####快捷方法：
    简化了复杂设置，只需要提供URL和HTTP方法（或方法中的数据）例如：
    - 1.get() 
    
      发送get（）请求
      
       接收两个参数，url和config（可选）。然后返回HttpPromise对象
     $http.get('data.json');
    - 2、delete（）
    
      发送删除请求 接收参数同上
      $http.delete('');
    - 3、head（）
      
      发送head（）请求 接收参数同上
      $http.head();
    - 4、jsonP()
    
     如果是JSONP请求，其中url必须包含JSON_CALLBACK,例如：
     
     $http.jsonp('data.json?callback=JSON_CALLBACK');
    - 5、post()
    
     发送post()请求，接收参数为三个，url，data，还有config（可选）。这里的data包含请求的数据。同样返回HttpPromise对象。
    - 6、put()

     发送put()请求，接收参数同上，data包含请求的数据。
  - ####设置对象
    当我们需要将$http作为函数来调用时，就要传入一个设置对象。如下：
        
          $scope.chuan=function(){
             $http({
               method:'get',
               url:'data.json',
               params:{
                 'username':'dahuang'
               }
            })
        }
        
  - ####缓存http请求
  
      默认情况下，$http服务不会对请求进行本地缓存，在发送单独的请求时，我们可以通过向$http请求发送一个布尔值或者一个缓存实例来启用缓存。
      
        $http.get('data.json'{cache:true})
             .success(function(data){})
             .error(function(err){})
  第一次发送请求时,$http会向data.json发送get请求，第二次发送同一个get请求时，$http会从缓存中取回请求的结果。
- ##使用$resouce
 
  他可以将$http的请求转换成save和update等简单的形式。
  
      安装：
      由于ngResouce不是默认内置在angularjs中的模块，因此我们要安装并引用他。
      1、下载并引用
      下载地址：http://code.angularjs.org
      2、用bower来安装
      $bower install --save angular-resouce
      这个模块需要在angualrjs之后引用
      <script src="angular.js"></script>
      <script src="angular-resource.js"></script>
      最后，依赖注入angular.module('myApp', ['ngResource']);
  
  我们并不是直接通过$resource服务本身同服务器通信，$resource是一个创建资源对象的工厂，用来创建同服务端交互的对象。
  
      var User = $resource('/api/users/:userId', {userId:'@id'});
      
      User.get({id:'123'}, successFn, errorFn);
      该方法向url发送一个get请求，并期望一个json类型的响应。这里会向/api/users/123发送一个请求，successFn处理请求成功响应，errorFn处理错误。

      User.query(params, successFn, errorFn)
      同get()方法使用类似，一般用来请求多条数据。

  返回的User对象包含了同后端服务进行交互的方法，我们可以把User对象理解成同RESTFul的后端服务进行交互的接口。
  
  通过$resource生成的对象来同服务器进行交互的时候，我们看可以定义处理成功以及处理失败的函数，这些函数接受的参数不仅仅是简单的对象，而是经过包装之后的对象，会被添加$save(), $remove(), $delete三个方法，可以直接调用这三个方法来后服务端进行交互。

      save(params, payload, successFn, errorFn);
      save方法会发起一个post请求，params参数用来填充url中变量，对象payload会作为请求体进行发送

      delete(params, payload, successFn,errorFn)
      delete方法一个DELETE请求，payload作为消息体进行发送

      remove(params, payload, successFn, errorFn)
      同delete类似，不同的是remove用来移除多条数据

  举个例子：

      angular.module('phonecatServices', ['ngResource']).
          factory('Phone', function($resource){
            return $resource('phones/:phoneId.json', {}, {
              query: {method:'GET', params:{phoneId:'phones'}, isArray:true}
            });
      });
      
     在controller中调用：
      function PhoneListCtrl($scope, Phone) {
        $scope.phones = Phone.query();
        $scope.orderProp = 'age';
      }
    
    $resouce服务使得用短短的几行代码就可以创建一个RESTful客户端。我们的应用使用这个客户端来代替底层的$http服务。
    
      $http.get('phones/phones.json').success(function(data) {
        $scope.phones = data;
       });
       换成了
      $scope.phones = Phone.query();
  
- ##Restangular
前两个angualrjs服务在某些方面的功能有限，Restangular集成了两者的优点，既能像$http方法那样链式调用，又能像$resouce那样简单的同时操作promise和对象。而且，$resource要求明确的制定要拉取数据的URL，Restangular并不需要（除了基础url以外）。Restangualr没有像$resouce里面的bug，类似url中的一些／ ：等问题。
  
  Restangular可以直接将ETags和if-None-Match用到你的请求中。ETags是用于标示url对象是否发生改变。区分不同语言和session等。具体的neibu含义是服务器控制的。
  这两个东西是干嘛的呢
  我们输入URL后，我们的浏览器给Web服务器发送了一个Request, Web服务器接到Request后进行处理，生成相应的Response，然后发送给浏览器， 浏览器解析Response中的HTML,这样我们就看到了网页

  我们的Request 有可能是经过了代理服务器，最后才到达Web服务器的。代理服务器就是网络信息的中转站，有什么功能呢？
  
　　1. 提高访问速度， 大多数的代理服务器都有缓存功能。

　　2. 突破限制， 也就是翻墙了

　　3. 隐藏身份。

 If-None-Match和ETag一起工作，工作原理是在HTTP Response中添加ETag信息。 当用户再次请求该资源时，将在HTTP Request 中加入If-None-Match信息(ETag的值)。如果服务器验证资源的ETag没有改变（该资源没有更新），将返回一个304状态告诉客户端使用本地缓存文件。否则将返回200状态和新的资源和Etag.使用这样的机制将提高网站的性能
 
####怎样添加到项目中


1. `bower install restangular`
2. `npm install restangular`
-  使用cdn  https://cdnjs.com/libraries/restangular/

####依赖

依赖`angular`和`lodash`

####入门指南

- `restangular`作为依赖项添加到你的`app`中  这里的`restangular`是小写

      angular.module("your-app",['restangular']);

- 把`restangular`注入到控制器中 以便使用  这里**注意**`Restangular`开头字母大写

      angular.module("your-app")
      .controller("oneCtr",function($scope,Restangular));

  例如：
      Restangular.all('users').getList()
                  .then(function(users){
                     $scope.user=users[0];   //返回的是一个用户列表
                     })
       可以在他之后继续获得他里面的数据
            $scope.user.getList('cars');     //GET:/users/123/cars
       也可以用自定义方法来操作数据
            $scope.user.sendMessage();    //POST:/users/123/sendMessage
            
####链式调用
      $scope.user.one('messages',123)
                  .one('from',123).getList('unread');  GET:/users/123/messages/123/from/123/unrea

####Restangualr对象调用的方法
      1 .one(route,id);
      这里将创建一个新的restangular对象 指向指定的路线和id
      
      - 如果是从数据库请求数据，则括号里有两个参数，一个是数据库名，一个是查询的匹配字段，此时后面跟着get()方法
      - 如果是从接口请求数据，则括号里是接口名
      
      one()后面如果是post()方法，则post()括号中需要传入三个参数，前两个根据情况 可以为null，第三个参数是object
      例如：
          one("接口名").post(null,null,object);
          
      one()后面如果是get()方法，则get()括号中需要传入两个参数，第一个可以为null，第二个参数是一个object
      例如：
          one("接口名").get(null,object);
          
      2 .all(route); 
      这里创建一个新的restangular对象 它是一个列表  指向指定元素
      也就是括号里写数据库的名字
      
      后面如果是getList()方法，括号里没有参数，获取的就是数据库中所有的数据
      如果getList()方法，括号里有参数，filters是过滤的条件
      例如：getList({
            'page_no':1,
            'page_size':5,
            'sort_item':'name',
            'sort_order':'desc',
            'filters:JSON.stringify({表名:{name:{like:'%'+mm+'%'}}
            })'
          })
      过滤项里的表名是必须项
      like中间的mm是需要过滤的模糊的条件 放在两个%中间 用+连接
      
      3 .oneurl(route,url);  
      创建一个新的restangular对象 指向指定的元素和指定的url
      4 .allurl(route,url);  
      与one的区别是 指向指定的列表和指定的url
      5 .copy(fromElement);  
      从一个元素那里复制创建一个副本，可以对这个副本进行操作

####常用元素方法
    get();  获取元素，查询参数 和标题头是可选项
    getList();  获取所有信息 返回一个集合 其中包含了可以操作特定集合的方法
    post();  接受一个必要参数，参数类型是对象，并向指定的url发送一个post请求
         我们也可以向请求中添加查询参数和头
         例如：var all=Restangular.all('messages');
              var new={body:'hello'}
              all.post(new)
              
*Restangular返回的是增强过的promise对象，因此除了可以调用then方法，还可以调用一些特殊的方法，比如$object  $object会返回一个空数组（或对象），在服务器返回信息后  数组会被用新的数据填充。这对更新一个集合后，在作用域中立即重新拉去集合的场景很有用*

         例如：all.post(new).then(function(item){
                //首先将消息设置成空数组
                //然后一旦getList是完整的就填充它
                $scope.message=all.getList().$object;
              },function(error){
                //出现了一个错误
              )
  *我们也可以从集合中移除一个对象 使用`remove()`方法可以发送一个DELETE HTTP 请求给后端 通过调用集合中一个对象的`remove()`方法来发送删除请求*
  
        Restangular中不仅可以在one和all方法创造的对象上调用post、getList等方法，
        也可以在服务器返回的对象上调用  
        例如：
            Restangular.one('authore','ab123')
                      .then(function(author){
                        $scope.author=author;
                      })
                  //然后继续请求
             $scope.author.getList('books');
               
               











