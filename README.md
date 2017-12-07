##两种启动方式 

 1 自动启动：通过 ng-app 指令
 2 手动启动：angular.bootstrap(document, ['MyModule'])

    angular
      .module('app1', [])
      .controller('TestController', function ($scope) {
        $scope.txt = '初始值';
      });
    
    // 手动启动ng
    angular.bootstrap(document, ['app1']);
    
####ng-app

 作用：启动ng，指令指定了应用的根元素（边界），只有在ng-app内部的标签才受到ng管理
 注意：默认情况下，只有第一个ng-app生效
   第一个：页面从上往下的第一个

###angular初体验

- demo：Hello angularjs


    <!-- 2 设置 ng-app 指令 -->
    <div ng-app>
      <!-- 3 给文本框添加 ng-model 指令 -->
      <input type="text" ng-model="name">
    
      <!-- 4 给 h1 添加插值表达式 -->
      <h1>{{ name }}</h1>
    </div>
    
    <!-- 1 引入angularjs -->
    <script src="./angular-1.6.6.js"></script>
    

###插值表达式

- 语法：{{}}
- 说明：可以用作标签的属性

    <p>{{ 1 > 2 ? '大于': '小于等于' }}</p>
    <p>{{ user.name }}</p>
    <p>{{ 'hello' + 'world' }}</p>
    
    <a href="{{ title }}"></a>

###directive 指令

 1.是HTML标签上的一些标记，以ng-开头，用来给HTML标签添加一些特殊的行为
 2.让 Angular 按照我们预先设置好的规则办事


    <!-- ng-model 指令 -->
    <input type="text" ng-model="userName">
    

###ng-model

 作用：将表单元素与数据绑定在一起，在 input/select/textarea 等表单元素中使用

    <input type="text" ng-model="name">

###ng-click

 作用：绑定事件
 说明：其他事件指令都是类似的语法，ng-mouseenter / ng-dblclick / ng-submit
 案例：加法计算器
 

    <!-- ng-click 的值：方法 -->
    <button ng-click="calc()"></button>
    
    <!-- ng-click 的值：表达式 -->
    <button ng-click="val + 1"></button>


###模块

 ng采用模块化开发方式
 ng中模块是基础，ng中所有内容都必须挂载到一个模块中

    模块是一个容器包含了应用程序的不同组成部分
      例如：controller, service, filter, directive, config 等
    
    模块是应用程序的组成单元，例如：登录模块、注册模块、商品列表模块 等，这些模块
    组合在一起构成了一个完整的应用程序。

###创建模块

 作用：创建一个模块，让AngluarJS对整个内容进行模块化管理
 

    // 第一个参数：模块名称
    // 第二个参数：数组，用来添加当前模块的依赖项，即使没有依赖项也要提供一个空数组
    
    // 没有依赖项的模块
    var app = angular.module('myApp', [])
    
    // 带有依赖项的模块
    angular.module('dependence1', [])
    angular.module('dependence2', [])
    var app1 = angular.module('myApp', ['dependence1', 'dependence2'])
    

###获取模块 

 语法：var app = angular.module(moduleName)
 作用：获取指定的模块

###controller 控制器

 需要配合ng-controller指令来使用

###创建控制器

 作用：通过$scope为视图提供数据和方法
 

    // 第一个参数：控制器名称
    // 第二个参数：函数，为应用添加数据和方法
    app.controller('DemoController', function($scope) {
      // $scope 表示当前的数据模型
      $scope.name = 'jack'
    });
    

###$scope的说明

 1 注意：$scope名称是固定写法
 2 模式：给 $scope 添加数据，在视图中就可以使用这些数据了
 3 推荐：给 $scope 添加数据应该使用对象，而不是作为其属性

###解决页面闪烁问题

 说明：在HTML中使用的插值表达式会经历从 {{ name }} 到 name的值 过程
 方式一： 将引用angularjs文件放到head中
 方式二： 使用 ng-bind 指令
 方式三： 使用 ng-cloak 指令

###ng-bind 指令

 作用：设置元素的 textContent
 注意：会覆盖标签中已存在的内容


    <p ng-bind="name"></p>


###ng-cloak 指令

 作用：用来解决表达式闪烁问题
 原理：1 通过CSS隐藏带有{{}}的元素 2 ng加载完成后会移除ng-cloak指令
 使用场景：页面中存在大量表达式


    <style>
      [ng-cloak] {
        display: none;
      }
    </style>
    
    <p ng-clock>{{name}}</p>
    

###常用指令介绍

####ngSanitize 模块

 语法： ng-bind-html="<div></div>"
 作用： 在页面中输出 html内容（innerHTML的功能）


    <div ng-bind-html="content"></div>
    <script src="angular-sanitize.js"></script>
    <script>
      // 引入 ngSanitize 模块
      var app = angular.module('testApp', ['ngSanitize'])
    
      app.controller('testController', ['$scope', function($scope) {
        $scope.content = '<h1>雨啊雨</h1>'
      }])


###ng-repeat 指令

 作用：遍历集合中的数据，为集合中的每条数据创建一个当前元素（即，带有指令的元素）
 说明：功能类似于 for-in 循环


    <ul>
      <li ng-repeat="item in list"></li>
    </ul>
    
    <script>
    app.controller('TestController', ['$scope', function($scope) {
      $scope.list = [
        {name: 'jack', age: 19},
        {name: 'tom', age: 21},
        {name: 'rose', age: 22}
      ];
    }]);
    
    

#### 使用 track by $index 解决，数据重复的问题

    <ul>
      <li ng-repeat="item in datas track by $index"></li>
    </ul>

ng-repeat 的循环项属性

 $odd/$even，用来表示当前项的奇偶性，类型为：布尔值
 $first/$last/$middle，用来表示当前项的位置，类型为：布尔值
 $index，用来表示当前项的索引号，从0开始计算

    <ul>
      <!-- 隔行变色效果的实现 -->
      <li ng-repeat="item in list" class="{{$odd ? 'red' : 'green'}}"></li>
    </ul>

###ng-class指令

 示例：ng-class="{red: $odd, green: $even}"
 解释：ng-class通过指定一个对象（对象字面量），键为：类名，值为：布尔值
 作用：对象中属性的值，为true则添加与该属性名相同的类，否则不添加

    <div ng-class="{active: true, cls: false}"></div>

###ng-hide/ng-show 显示和隐藏

 作用：控制当前元素的展示和隐藏，类型为：布尔值
 语法：ng-show='布尔值'
 隐藏元素是通过 css 样式 display: none

    <div ng-show="isShow"></div>

###ng-if

 作用：控当前元素的显示或隐藏状态，这里的隐藏指的是：页面中不存在当前元素
 语法：ng-if="布尔值"

    <div ng-if="false"></div>

###ng-switch

 作用：类似于js中的switch-case，配合ng-switch-when来使用
 当 ng-switch 的值与 when 的值相同，那么该项就会展示

    <div ng-switch="name">
      <div ng-switch-when="jack">我是jack</div>
      <div ng-switch-when="tom">我是tom</div>
      <div ng-switch-when="rose">我是rose</div>
    </div>
    
    <script>
      $scope.name = "jack"
    </script>

###$watch 监听数据

 语法：$scope.$watch(attrName, callback, flag);
 作用：监听$scope中数据模型的变化，无法监视其他的数据（例如，普通变量）
 如果要监视对象属性的变化，需要将第三个参数设置为：true
 注意：调用$watch方法时，会立即被调用一次

    app.controller('demoController', function($scope) {
      $scope.name = 'jack';
      $scope.user = {
        age: 19
      }
    
      // 参数一：表示监听的$scope中的属性名称，类型为：字符串
      // 参数二：表示数据变化执行的回调函数，有两个参数分别是：当前值与变化之前的值
      // 参数三：比较方式，false：表示比较引用；true：表示比较值。默认值为false
      $scope.$watch('name', function(curValue, oldValue) {
        // 只要被监听的数据发生变化，就会指定该回调函数中的代码！
    
        // 略过第一次执行
        if(curValue === oldValue) return;
      });
    });
    
    // 监视对象属性的改变
    $scope.$watch('user', function(curValue, oldValue) {
    }, true);






