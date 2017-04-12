# Lodash

#### 作者：申海东

#### 邮箱：shenhaidong@haomo-studio.com

参考文档：[http://lodashjs.com/docs/](http://lodashjs.com/docs/)

## lodash简介：

lodash是一款接口统一、模块化、高性能的小型JavaScript工具库；

* 最新版本支持≥IE9、chrome、firefox、≥Safari9以及node.js6-7版本；

* lodash是一个高度模块化的工具，可以按需加载；

* lodash是使用了惰性计算，提高程序性能；

* 像jquery里面的$一样，lodash使用了全局的\_来提供对工具的快速访问


## lodash发展

- 随着前端的兴起和发展，尤其是node.js的兴起，原生javascript对于数据处理方法不能满足人们的需求，像jquery对dom，bom的操作，大大简化了前端人员的工作量；于是，前端工作人员开始私人封装方法，并不断发展成自己的处理数据的库，在这种情况下，underscore.js和lodash.js应运而生，从而大大简化了处理数据（像number，数组，字符串，对象等）工作。

- lodash.js原来作为underscore.js的一个fork，由于其与其他的underscore的贡献者意见不一致，John-David Dalton旨在创建“跨浏览器的行为一致的，高性能的”js工具库，从而在原有成功基础上取得了更大的成就。

## 发展前景

- lodash是一款“一直接口，模块化，高性能的javascript”库。lodash不仅有所有的underscore的方法，当underscore.js更新方法，lodash会马上将其移植到自己的项目中，并且lodash还包括了很多underscore没有的方法，又由于其高性能，模块化等优势，现在lodash已经成为npm仓库中依赖最多的包。现在我们所熟知的很多开源项目都已经使用或者转到了lodash阵营之上。比如JavaScript转译器Babel、博客平台Ghost，和项目脚手架工具Yeoman。

- lazy.js是一个新的类lodash的库，其优异的运算性能，但Lodash不同于他的地方主要在于你仍然拥有一个外表和Underscore一样的API，但内部却是一个新的强大的引擎。不需要学习新的库，不需要更改大量的代码，就像只是一个附加的更新一样。而且惰性计算也会引起其他潜在的性能问题，我们将在结尾讨论；

- **data:** npm官网提供的最近一个月（16年11月11日到16年12月10日）lodash的下载次数：37,063,269 ；而同期的underscore的下载量只有：11,011,512 。由于历史遗留问题，很多underscore的拥护者仍将使用underscore.js，但是lodash缓慢替代underscore的趋势是不可避免的。

**总结：** 作为npm下载量最大的js依赖库，lodash.js在我们的工作中必将发会越来越重要的作用，熟练使用lodash.js将为我们的工作带来很大便利。

lodash的挑战：随着ES6规范的普及，lodash的一些方法，可能别js原生方法替代，比如_.map,_.time,\_.random等等；

![](/assets/9vP6sVG.png)

## 入门指南

### 安装：

* 下载lodash：npm install lodash ；

* HTML使用script标签引入。

* npm安装：

  ![](/assets/屏幕快照 2016-11-22 上午9.59.29.png)

* node.js使用：

  ![](/assets/屏幕快照 2016-11-22 上午10.02.03.png)




### 常用方法：

**本文的方法实例使用的lodash.js版本为4.17.3,可以在使用CDN引入或者下载新版本**

1. \_.times\(\_.times\(number,function\)\)

   相对于for循环，lodash提供了更为高效的循环方法：\_.times\(number,function\); 该方法会返回一个数组；

            var i = 0;
            var time1 = _.times(3, function(){
                console.log(i++);
                return i;
            });
            // => 输出结果为：0, 1, 2

            var time2 = _.times(4, _.constant(0));
            console.log(time1, time2);
            // => 输出结果为： [1, 2, 3]    [0, 0, 0, 0]

  使用_.times方法创建一个有相同前缀的值的数组；

            var newArr = _.times(6, _.partial(_.uniqueId, 'time_'));
            console.log(newArr);
            // => ["team_1", "team_2", "team_3", "team_4", "team_5", "team_6"];

2. \_.filter\(array,fucntion\(item\){return //判断条件}\)

   筛选符合条件的数组子项；返回新数组，原数组不变
  
              var obj = {'data': [
                {
                    'category': {
                        'uri': '/categories/0b092e7c-4d2c-4eba-8c4e-80937c9e483d',
                        'parent': 'Food',
                        'name': 'Costco'
                    },
                    'amount': '15.0',
                    'debit': true
                },
                {
                    'category': {
                        'uri': '/categories/d6c10cd2-e285-4829-ad8d-c1dc1fdeea2e',
                        'parent': 'Food',
                        'name': 'India Bazaar'
                    },
                    'amount': '10.0',
                    'debit': true
                },
                {
                    'category': {
                        'uri': '/categories/d6c10cd2-e285-4829-ad8d-c1dc1fdeea2e',
                        'parent': 'Food',
                        'name': 'Sprouts'
                    },
                    'amount': '11.1',
                    'debit': false
                }
                ]};
                console.log(_.filter(obj.data,'debit'));
                // =>
                //[
                //    {
                //        'category': {
                //            'uri': '/categories/0b092e7c-4d2c-4eba-8c4e-80937c9e483d',
                //            'parent': 'Food',
                //            'name': 'Costco'
                //        },
                //        'amount': '15.0',
                //        'debit': true
                //    },
                //    {
                //        'category': {
                //            'uri': '/categories/d6c10cd2-e285-4829-ad8d-c1dc1fdeea2e',
                //            'parent': 'Food',
                //            'name': 'India Bazaar'
                //        },
                //        'amount': '10.0',
                //        'debit': true
                //    }]

                console.log(_.filter(_.filter(obj.data), function(item){
                    return item.amount == 11.1;
                }));
                //    {
                //        'category': {
                //        'uri': '/categories/d6c10cd2-e285-4829-ad8d-c1dc1fdeea2e',
                //                'parent': 'Food',
                //               'name': 'Sprouts'
                //   },
                //        'amount': '11.1',
                //            'debit': false
                //    }

3. \_.reject\(array,function\(item\){return //判断条件}\)

   取反，返回不符合判断条件的新数组，原数组不变

4.  \_.remove\(array,function\(item\){return //判断条件}\)

   去除符合条件的数组子项，改变原数组，返回被移除的子项组成的新数组
   
           var array = [1, 2, 3, 4];
           var evens = _.remove(array, function(n) {
             return n % 2 == 0;
           });

           console.log(array);
           // => [1, 3]
           console.log(evens);
          // => [2, 4]

5. \_.omit\(obj,arr\)；

   一个新的对象，移除了与数组项相同的属性；

        var omit = {
            'name': 'lijinglin',
            'height': 175,
            'weight': '95kg'
        }
        var newOmit = _.omit(omit, ['name','height']);
        console.log(omit, newOmit);
        // {'name': 'lijinglin', 'height': 175, 'weight': '95kg'} =>原数组不变
        // {'height':175} =>清除了数组项；

6. \_.pick\(obg,function\(item\){}\);

   一个新的对象，包含与数组项相同的属性；

        var objA = {"name": "colin", "car": "suzuki", "age": 17};
        console.log(_.pick(objA, ['car', 'age']));
        // => {"car": "suzuki", "age": 17}

7. \_.random\(min,max,[floating](boolean)\);

   返回min-max之间的随机数，如果只有一个参数，那么返回0-该值之间的随机数，最后的参数为boolean值，表示返回的数是否是浮点数；

        var random = _.random(10);
        var random1 = _.random(10,20);
        var random2 = _.random(10,20,true);
        var random3 = _.random(10,20,2);
        console.log(random, random1, random2, random3);
        //输出：
        // 10
        // 16
        // 13.626099468086885
        // 15.984136145057482

        var random4 = _.round(random2, 2);
        console.log(random4);
        // 输出：
        // 13.63

8. \_.map\(Collection,functoion\|\|string\);

   返回一个数组。处理数组对象，当第二个参数为function改变每一项，该方法可以用于深层查找属性值；**\_.map**方法是对原生 map 方法的改进，其中使用 **pets\[0\].name** 字符串对嵌套数据取值的方式简化了很多冗余的代码，

   ![](/assets/屏幕快照 2016-11-23 下午4.10.37.png)

* \_.each(Collection,function(value,(key)){})  

  该方法用于数组或者对象的遍历，可以完美的代替for循环；第一个参数为需要遍历的数组或者对象，function里面的参数第一个value值必传，代表数组或对象中的每一项的值，key参数可以根据自己需求传递，表示数组或者对象中的每一项的键（key）；

  _.each()方法中的continue和break：
  
  在该方法中实现for循环break功能；只需要在方法结束时判断是否需要返回：eg：

       var arr = [1,2,3,4,5,6,7,8];
        _.each(arr, (item, key) =>{
            console.log(item);
            if(key == 3) return false;
        });
        // => 输出1，2，3，4
        // => 效果和for循环break一致

        var obj = [
            {'name': 'lion', 'age': '17'},
            {'name': 'lina', 'age': '18'},
            {'name': 'luna', 'age': '23'},
            {'name': 'lich', 'age': '14'}
        ];
        _.each(obj, (item) => {
            if(item.age >= 18) return true;
            console.log(item.name + '未成年');
        });
        // => 输出：lion未成年；lich未成年；
        // => 效果和for循环的continue一致；

> lodash更多方法请参考lodash文档：  
> [lodashAPI](https://lodash.com/docs/)
>
> 实用的lodash中文网站：[http://www.itdadao.com/tags/lodash-0.html](http://www.itdadao.com/tags/lodash-0.html)

## 前端常用工具：

1. **is.js** 为检查数据类型等提供的小型的js库，大大减少了我们对REGEXP的依赖；

2. **underscore.js**和**lodash.js** 提供了一整套工具函数，无需经验不足的程序员再去给内置的 JavaScript 对象打补丁；

3. **D3.js** 数据可视化和图表是web应用程序的一种常规需求。当涉及到任何数据操作和可视化时，D3.js 就是事实上的标准。它是 GitHub 上最受欢迎的项目之一，并被数百个组织机构所采用。大量的图形、图标和可视化库都是构件于 D3 之上的；[D3.js网站](https://D3js.org/)

4. **three.js** Three.js 提供了一个轻量级的 3D 库，让你可以将 3D 效果渲染成一个 HTML5 的 canvas, SVG, 和 WebGL。在   
   [three网站](https://threejs.org/) 上展示了数百个漂亮的实例；

5. **lazy.js**  Lazy.js是类似Underscore或Lo-Dash的JavaScript工具库,但是它有一个非常独特的特性:惰性求值，具有优良的计算性能；但是惰性计算会带来其他方便的性能问题，详见**注释**；故而使用惰性计算需要慎重考虑。

###注释

<hr/>

  - 惰性计算(Lazy Evaluation)

#### 惰性计算是什么

  - 函数的参数只有在需要计算是才会被计算；
  - 一个函数不需要完全被计算只有需要的部分会被计算；
  - 一个参数只会被计算一次；


惰性计算可以通过避免不必要的计算从而增加程序运行速率；

  #### 惰性计算的不足

  - 由于必要的参数或则计算才会被使用，所以惰性计算在运行时，会首先判断是否需要计算，或者是否存在，从而造成另外不必要的开销；
  这会导致在运行一些小的计算时，使得惰性计算并没有必要。
  - 惰性计算在一定程度上可能造成内存泄露；
  解释：惰性计算会将所有的参数计算一次，那么惰性计算会将所有计算的参数保存在内存中，当该参数不再使用后仍然可能会占用内存，这在一定程度堵上造成内存泄露的风险。

<hr/>

####前端中的钩子使用：

  百度中钩子函数的定义：

 钩子函数：钩子函数是Windows消息处理机制的一部分，通过设置“钩子”，应用程序可以在系统级对所有消息、事件进行过滤，访问在正常情况下无法访问的消息。钩子的本质是一段用以处理系统消息的程序，通过系统调用，把它挂入系统。

  我们要讨论的是钩子在lodash中的应用：

#### 我理解的前端中的钩子：

  - 前端中的钩子就是在函数或者页面调用之前对部分函数或页面内的方法进行了统一的处理，从而达到低耦合高内聚的目的；
  
  - **耦合**：一个软件结构内不同模块之间互连程度的度量。

  - **内聚**：一个模块内部各个元素彼此结合的紧密程度的度量。

####  lodash中hook的使用实例：

 - #####惰性计算的判断：

   在lodash中，处理复杂的对象或者较大的数组时会使用lazy计算从而提高程序的运行性能；在调用lodash的运行方法之前例如

  1. 数组、对象、collection方法中的
     * `at`, `compact`, `drop`, `dropRight`, `dropWhile`, `filter`, `find`, `findLast`, `head`, `initial`, `last`, `map`, `reject`, `reverse`, `slice`, `tail`, `take`, `takeRight`, `takeRightWhile`, `takeWhile`, and `toArray`；

  2. 以及_.chain()方法中的
     * `after`, `ary`, `assign`, `assignIn`,`assignInWith`, `assignWith`, `at`,
`before`, `bind`, `bindAll`, `bindKey`, `castArray`, `chain`, `chunk`,`commit`, `compact`, `concat`, `conforms`, `constant`, `countBy`, `create`, `curry`, `debounce`, `defaults`, `defaultsDeep`, `defer`, `delay`， `difference`, `differenceBy`, `differenceWith`, `drop`, `dropRight`,`dropRightWhile`, `dropWhile`, `extend`等方法；

  在调用这些方法之前会通过islaziable(func){}(版本号4.17.2第6338行)来对这些方法进行处理，从而计算出是否需要惰性计算，这些是在内部实现的，可以预处理lodash的绝大多数方法；从而增强了lodash的内聚度，而在外部函数中可以随意调用，而在外部随意调用，不要再处理。从而实现外部模块对随意调用。降低了耦合度。

- ##### lodash collection方法的使用：

  collection方法中可以传入数组或者对象，lodash会通过getFuncName（）从而计算出你传入的对象，实现对目标对象的解析。大大增强了lodash中collection的方法的灵活性。


 


 






