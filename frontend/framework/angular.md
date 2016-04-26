# 简介

本风格指南的目的是展示AngularJS在OmniSocials应用的最佳实践和风格指南，语言风格指南[参考]()

# 内容目录
* [概览](#概览)
    * [目录结构](#目录结构)
    * [标记](#标记)
    * [命名约定](#命名约定)
    * [其他](#其他)
* [模块](#模块)
* [控制器](#控制器)
* [指令](#指令)
* [过滤器](#过滤器)
* [服务](#服务)
* [模板](#模板)
* [路由](#路由)
* [国际化](#国际化)
* [性能](#性能)

# 概览

## 目录结构

由于一个大型的AngularJS应用有较多组成部分，所以我们是通过拆分模块的方式来组织的。模块分为核心模块和业务模块，核心模块只有一个叫做`core`，其他在模块结构中的均为业务模块

* 总体的portal后台管理前端代码结构如下：

```
static/portal
    ├── coffee
    │   ├── app.coffee
    │   ├── bootstrap.coffee
    │   ├── config.coffee
    │   └── main.coffee
    ├── modules
    │   ├── analytic
    │   ├── channel
    │   ├── content
    │   ├── core
    │   ├── ...
    │   └── store
    └── scss
        ├── app.scss
        ├── bootstrap
        ├── _dynamic-variables.scss
        ├── loader.scss
        └── _variables.scss
```

* 核心模块按照类型优先的组织方式目录结构看起来如下：

```
static/portal/modules/core
    ├── controllers
    │   ├── graphicCtrl.coffee
    │   ├── ...
    │   └── userCtrl.coffee
    ├── coreLoader.coffee
    ├── coreModule.coffee
    ├── directives
    │   ├── osAutoComplete.coffee
    │   ├── ...
    │   └── osSelect.coffee
    ├── filter
    │   ├── countdownFilter.coffee
    │   ├── ...
    │   └── trustAsHtmlFilter.coffee
    ├── i18n
    │   ├── locate-en_us.json
    │   └── locate-zh_cn.json
    ├── index.scss
    ├── meta
    │   ├── sub-location.json
    │   └── three-location.json
    ├── partials
    │   ├── editButtonLinkModal.html
    │   ├── ...
    │   ├── spreadByDownloadQrcode.html
    │   └── wm-bootstrap-tpls.js
    ├── services
    │   ├── authService.coffee
    │   ├── ...
    │   └── validateService.coffee
    ├── styles
    │   ├── components
    │   ├── _components.scss
    │   ├── _layout.scss
    │   └── mixins
    ├── sub-location
    │   ├── locate-en_us.json
    │   └── locate-zh_cn.json
    └── three-location
        ├── locate-en_us.json
        └── locate-zh_cn.json
```

* 业务模块按照业务优先的组织方式目录结构看起来如下：

如下：

```
static/portal/modules/member
    ├── config.json
    ├── controllers
    │   ├── analyticCtrl.coffee
    │   ├── ...
    │   └── previewCtrl.coffee
    ├── i18n
    │   ├── locate-en_us.json
    │   └── locate-zh_cn.json
    ├── index.scss
    ├── introduction.html
    ├── introduction.json
    ├── partials
    │   ├── analytic.html
    │   ├── ...
    │   └── preview.html
    └── styles
        ├── analytic.scss
        ├── ...
        └── setting.scss
```

* 当目录里有多个单词时, 使用 lisp-case 语法:

```
static/portal/modules/my-complex-module/
    ├── config.json
    ├── controllers
    ├── i18n
    ├── index.scss
    ├── introduction.html
    ├── introduction.json
    ├── partials
    └── styles
```

## 标记

保持标签的简洁并把AngularJS的标签放在标准HTML属性后面。这样提高了代码可读性。标准HTML属性和AngularJS的属性没有混到一起，提高了代码的可维护性。

```html
<form class="frm" ng-submit="login.authenticate()">
  <div>
    <input class="ipt" type="text" placeholder="name" require ng-model="user.name">
  </div>
</form>
```

其它的HTML标签应该遵循下面的指南的 [建议](http://mdo.github.io/code-guide/#html-attribute-order)

## 命名约定

下表展示了各个Angular元素的命名约定

元素 | 命名风格 | 实例 | 用途
----|------|----|--------
Modules | lowerCamelCase  | angularApp |
Controllers | lowerCamelCase + 'Ctrl'  | userCtrl |
Directives | lowerCamelCase  | userInfo |
Filters | lowerCamelCase | userFilter |
Factory | lowerCamelCase | canvasService | others

## 其他

* 使用：
    * `$timeout`  替代 `setTimeout`
    * `$interval` instead of `setInterval`
    * `$window`   替代 `window`
    * `$document` 替代 `document`
    * `$http`     替代 `$.ajax`

这将使你更易于在测试时处理代码异常 (例如：你在 `setTimeout` 中忘记 `$scope.$apply`)

使用如下工具自动化工作流
    * [Grunt](http://gruntjs.com)
    * [Bower](http://bower.io)

* 使用 promise (`$q`) 而非回调。这将使你的代码更加优雅、直观，并且免于回调地狱。
* 尽可能使用 `$resource` 而非 `$http`。更高的抽象可以避免冗余。
* 使用AngularJS的预压缩版 (像 [ngmin](https://github.com/btford/ngmin) 或 [ng-annotate](https://github.com/olov/ng-annotate)) 避免在压缩之后出现问题。
* 不要使用全局变量或函数。通过依赖注入解决所有依赖，这可以减少 bug ，规避很多测试时的麻烦。
* 为避免使用全局变量或函数，可以借助 Grunt 或 Gulp 把你的代码放到一个立即执行的函数表达式（IIFE）中。可用的插件有 [grunt-wrap](https://www.npmjs.com/package/grunt-wrap) 或 [gulp-wrap](https://www.npmjs.com/package/gulp-wrap/)。下面是 Gulp 的示例：

```javascript
gulp.src("./src/*.js")
    .pipe(wrap('(function(){\n"use strict";\n<%= contents %>\n})();'))
    .pipe(gulp.dest("./dist"));
```

* 不要污染 `$scope`。仅添加与视图相关的函数和变量。
* [使用 controllers 而非 `ngInit`](https://github.com/angular/angular.js/pull/4366/files)。`ngInit` 只有在一种情况下的使用是合适的：用来给 `ngRepeat`的特殊属性赋予一个别名。除此之外, 你应该使用 controllers 而不是 `ngInit` 来初始化scope变量。`ngInit` 中的表达式会传递给 Angular 的 `$parse` 服务，通过词法分析，语法分析，求值等过程。这会导致:
    - 对性能的巨大影响，因为解释器由 Javascript 写成
    - 多数情况下，`$parse` 服务中对表达式的缓存基本不起作用，因为 `ngInit` 表达式经常只有一次求值
    - 很容易出错，因为是模板中写字符串，没有针对表达式的语法高亮和进一步的编辑器支持
    - 不会抛出运行时错误
* 不要使用 `$` 前缀来命名变量, 属性和方法. 这种前缀是预留给 AngularJS 来使用的.
* 当使用 DI 机制来解决依赖关系, 要根据他们的类型进行排序 -  AngularJS 内建的依赖要优先, 之后才是你自定义的：

```coffee
module.factory('Service', ($rootScope, $timeout, MyCustomDependency1, MyCustomDependency2) ->
  return # Something
)
```

# 模块

* 模块应该用驼峰式命名。为表明模块 `b` 是模块 `a` 的子模块, 可以用点号连接: `a.b` 。
* 项目中的主模块是`wm`，该模块依赖于核心模块和其他第三方模块:

  ```coffee
  app = ng.module('wm', [
    'ui.router'
    'ui.bootstrap.tpls'
    'ui.bootstrap'
    'pascalprecht.translate'
    'scs.couch-potato'
    'angularFileUpload'
    'pasvaz.bindonce'
    'ng.ueditor'
    'ngSanitize'
    'wm.core'
  ])
  ```

* `wm.core`模块通过coreLoader声明核心模块的依赖将常用的核心控制器，服务，过滤器以及指令打包成一个模块依赖(通过自定义的grunt中mergeamd任务实现)

## 控制器

### 标准的控制器模板

所有的控制器遵循统一的书写模板，示例如下：

```coffee
define [
  'wm/app'
  'wm/config'
], (app, config) ->
  DemoController = ($stateParams, restService) ->
    # 常量定义部分
    CONSTANT =
      TITLE: 'title'

    # 页面视图模型初始化部分
    init = (vm) ->
      vm.cardId = $stateParams.id

      initBreadcrumb vm
      initTable vm
      fetchList vm

　　　　# 页面视图模型依赖的初始化函数定义部分
　　　　#　初始化面包屑
    initBreadcrumb = (vm) ->
      vm.breadcrumb = [
        {
          text: 'customer_card',
          href: '/member/card'
        },
        'customer_card_detail'
      ]

　　　　#　初始化数据列表
    initTable = (vm) ->
      vm.list =
        columnDefs: [
          {
            field: 'name'
            label: 'demo_name'
            type: 'link'
          }
          {
            field: 'menuType'
            label: 'demo_type'
            type: 'translate'
          }
          {
            field: 'channel'
            label: 'demo_channel'
            type: 'iconText'
          }
          {
            field: 'hitCount'
            label: 'demo_hit_count'
            sortable: true
          }
        ]
        data: []
        linkHandler: tableClickHandler
        sortHandler: tableSortHandler

    # 跟服务器交互相关函数定义部分(可以定义到模型层)
    fetchList = (vm) ->
      restService.get config.resources.demo, (data) ->
        items = data?.items
        if items and angular.isArrary items and items.length
          vm.list = data.items
        else
          vm.list = []

    # 数据列表定义回调方法
    tableClickHandler = (idx) ->
      modalInstance = $modal.open(
        templateUrl: 'menu.html'
        controller: 'wm.ctrl.member.menu'
        windowClass: 'user-dialog'
        resolve:
          modalData: ->
            menu: vm.menuList.data[idx]
            openId: vm.openId
            channelId: vm.channelId
      ).result.then( (data) ->
        return
      )

    tableSortHandler = (colDef) ->
      key = colDef.field
      value = if colDef.desc then 'desc' else 'asc'
      vm.orderby = '{"' + key + '":' + '"' + value + '"}'
      vm.currentPage = 1
      _getList(vm.curTab)

    # 视图模型赋值部分
    vm = this

　　　 # 视图模型绑定方法定义部分
    vm.clickHandler = ->
      vm.showNav = not vm.showNav
      fetchList vm

    vm.focusHandler = ->
      vm.btnClass =
      fetchList vm

　　　　# 调用初始化方法
    init vm

    vm

　　# 注册控制器部分
  app.registerController 'wm.ctrl.module.demo', [
    '$stateParams'
    'restService'
    DemoController
  ]
```

### 一些注意点

* 控制器里面的方法不要添加`_`符号，所有的方式使用小写开头驼峰方式，控制器不存在暴露可见性的问题。
* 不要在控制器里操作 DOM，这会让你的控制器难以测试，而且违背了[关注点分离原则](https://en.wikipedia.org/wiki/Separation_of_concerns)。应该通过指令操作 DOM。
* 通过控制器完成的功能命名控制器 (如：购物卡，主页，控制板)，并以字符串`Ctrl`结尾。
* 控制器不应该在全局中定义，要使用命名空间命名核心或者业务控制器，遵循规则`wm.ctrl.moduleName.controllerName`。
* 由于使用[angular-couch-potato](https://github.com/laurelnaiad/angular-couch-potato)来对angular组件做延迟加载，在`app`实例上使用`registerController`方法注册控制器：

  ```coffee
  GraphicsCtrl = ($modal, restService) ->
    ...
    vm = this
    ...
    vm

  app.registerController 'wm.ctrl.content.graphics', [
    '$modal'
    'restService'
    GraphicsCtrl
  ]
  ```

* 使用 `controller as` 语法:

  ```html
  <div ng-controller="wm.ctrl.analytic.wechat.followersGrowth as followersGrowth">
     {{ followersGrowth.title }}
  </div>
  ```

  ```coffee
  FollowersGrowthCtrl = (restService, $modal) ->
      ...
      vm = this
      vm.title = 'Some title'
      ...
      vm

  app.registerController 'wm.ctrl.analytic.wechat.followersGrowth', [
    '$modal'
    'restService'
    FollowersGrowthCtrl
  ]
  ```

   使用 `controller as` 主要的优点是:
   * 创建了一个“独立”的组件——绑定的属性不属于 `$scope` 原型链。这是一个很好的实践，因为 `$scope` 原型继承有一些重要的缺点（这可能是为什么它在 Angular 2 中被移除了）：
      * Scope值的改变会在你不注意的地方有影响。
      * 难以重构。
      * [dot rule](http://jimhoskins.com/2012/12/14/nested-scopes-in-angularjs.html)'.
   * 当你不需要做必须由 `$scope` 完成的操作（比如`$scope.$broadcast`）时，移除掉了 `$scope`，就是为 Angular2 做好准备。
   * 语法上更接近于普通的 JavaScript 构造函数。

   想深入了解 `controller as` ，请看: [digging-into-angulars-controller-as-syntax](http://toddmotto.com/digging-into-angulars-controller-as-syntax/)
* 如果使用数组定义语法声明控制器，使用控制器依赖的原名。这将提高代码的可读性：

  ```coffee
  GraphicsCtrl = (m, r) ->
    ...
    vm = this
    ...
    vm

  app.registerController 'wm.ctrl.content.graphics', [
    '$modal'
    'restService'
    GraphicsCtrl
  ]
  ```

   下面的代码更易理解

  ```coffee
  GraphicsCtrl = ($modal, restService) ->
    ...
    vm = this
    ...
    vm

  app.registerController 'wm.ctrl.content.graphics', [
    '$modal'
    'restService'
    GraphicsCtrl
  ]
  ```

   对于包含大量代码的需要上下滚动的文件尤其适用。这可能使你忘记某一变量是对应哪一个依赖。

* 尽可能的精简控制器。将通用函数抽象为独立的服务。
* 不要在控制器中写业务逻辑。把业务逻辑交给模型层的服务。
  举个例子:

  ```coffee
  // 这是把业务逻辑放在控制器的常见做法
  OrderCtrl = ->
    ...
    vm = this
    vm.items = []

    vm.addToOrder = (item) ->
      vm.items.push(item) #控制器中的业务逻辑

    vm.removeFromOrder = (item) ->
      vm.items.splice(vm.items.indexOf(item), 1) #控制器中的业务逻辑

    vm.totalPrice = ->
      vm.items.reduce((memo, item) ->
        memo + (item.qty * item.price) #控制器中的业务逻辑
      , 0)

    vm

  app.registerController 'wm.ctrl.content.order', [
    '$modal'
    'restService'
    OrderCtrl
  ]
  ```

  当你把业务逻辑交给模型层的服务，控制器看起来就会想这样：（关于 service-model 的实现，参看 'use services as your Model'）:

  ```coffee
  // Order 在此作为一个 'model'
  OrderCtrl = (Order) ->
    ...
    vm = this
    vm.items = Order.items

    vm.addToOrder = (item) ->
      Order.addToOrder item

    vm.removeFromOrder = (item) ->
      Order.removeFromOrder item

    vm.totalPrice = ->
      Order.total()

    vm

  app.registerController 'wm.ctrl.content.order', [
    '$modal'
    'restService'
    OrderCtrl
  ]
  ```

  为什么控制器不应该包含业务逻辑和应用状态？
  * 控制器会在每个视图中被实例化，在视图被销毁时也要同时销毁
  * 控制器是不可重用的——它与视图有耦合
  * Controllers are not meant to be injected


* 需要进行跨控制器通讯时，通过方法引用(通常是子控制器到父控制器的通讯)或者 `$emit`, `$broadcast` 及 `$on` 方法。发送或广播的消息应该限定在最小的作用域。
* 制定一个通过 `$emit`, `$broadcast` 发送的消息列表并且仔细的管理以防命名冲突和bug。

   Example:

   ```coffee
   // app.coffee
   #　Custom events:
   #　  - 'authorization-message' - description of the message
   #    - { user, role, action } - data format
   #      - user - a string, which contains the username
   #      - role - an ID of the role the user has
   #      - action - specific ation the user tries to perform
   ```

* 在需要格式化数据时将格式化逻辑封装成 [过滤器](#过滤器) 并将其声明为依赖：

   ```coffee
   myFormatFilter = ->
     () ->
       # ...

   module.filter('myFormat', myFormatFilter);

   MyCtrl = (myFormatFilter) ->
     # ...

   app.registerController 'wm.ctrl.content.my', myCtrl
   ```
* 有内嵌的控制器时使用 "内嵌作用域" ( `controllerAs` 语法)：

   **contentCtrl.coffee**
   ```coffee
   define [
    'wm/app'
    'wm/config'
    'wm/modules/module/controllers/enterpriseCtrl'
   ], (app, config) ->
    ContentCtrl = (restService) ->
      ...
      vm = this
      vm.templatePath = './partials/enterprise.html'

    app.registerController 'wm.ctrl.analytic.content', [
      'restService'
      ContentCtrl
    ]
   ```
   **enterpriseCtrl.coffee**
   ```coffee
   EnterpriseCtrl = ->
     ...
     vm = this
     vm.bindingValue = 42
     ...
     vm

   app.registerController 'wm.ctrl.module.enterprise', [
     EnterpriseCtrl
   ]
   ```

   **enterprise.html**
   ```html
   <div ng-controller="wm.ctrl.module.enterprise as enterprise">
     <h1 ng-bind="enterprise.bindingValue"></h1>
   </div>
   ```

--------TODO: add project specific notes below----------

# 指令

* 使用小写字母开头的驼峰法命名指令。
* 在 link function 中使用 `scope` 而非 `$scope`。在 compile 中, 你已经定义参数的 post/pre link functions 将在函数被执行时传递, 你无法通过依赖注入改变他们。这种方式同样应用在 AngularJS 项目中。
* 为你的指令添加自定义前缀以免与第三方指令冲突。
* 不要使用 `ng` 或 `ui` 前缀，因为这些备用于 AngularJS 和 AngularJS UI。
* DOM 操作只通过指令完成。
* 为你开发的可复用组件创建独立作用域。
* 以属性和元素形式使用指令，而不是注释和 class。这会使你的代码可读性更高。
* 使用 `scope.$on('$destroy', fn)` 来清除。这点在使用第三方指令的时候特别有用。
* 处理不可信的数据时，不要忘记使用 `$sce` 。


# 过滤器

* 使用小写字母开头的驼峰法命名过滤器。
* 尽可能使过滤器精简。过滤器在 `$digest` loop 中被频繁调用，过于复杂的过滤器将使得整个应用缓慢。
* 在过滤器中只做一件事。更加复杂的操作可以用 pipe 串联多个过滤器来实现。


# 服务

这个部分包含了 AngularJS 服务组件的相关信息。下面提到的东西与定义服务的具体方式（`.provider`, `.factory`, `.service` 等）无关，除非有特别提到。

* 用驼峰法命名服务。
  * 用首字母小写的驼峰法命名单例的服务, 如果是公共的服务，必须以`Service`结尾，例如：

    ```coffee
    PageCtrl = (utilService) ->
      ...
      utilService.render()
      ...

    app.registerController('wm.ctrl.module.page', [
      'utilService'
      PageCtrl
    ])

    # Common Service
    mod.factory('utilService', ->
      util =
        render: ->
          # render logic
          return
    )
    ```

  * 用首字母大写的驼峰法命名其它所有的需要实例化的业务逻辑服务。

* 把业务逻辑封装到单例工厂服务中，把业务逻辑抽象为服务作为你的 `model`。例如：
  ```coffee
  # order is the 'model'
  app.registerFactory('order', ->
      order =
        items: []

      order.add = (item) ->
        @items.push (item)

      order.remove = (item) ->
        if @items.indexOf(item) > -1
          @items.splice(@items.indexOf(item), 1)

      order.total = ->
        @items.reduce((memo, item) ->
          memo + (item.qty * item.price);
        , 0)

      order
  )
  ```

  如果需要例子展现如何在控制器中使用服务，请参考 '不要在控制器中写业务逻辑'。
* 将业务逻辑封装成 `service` 而非 `factory`，service仅适用于需要实例化的`model`，这样我们可以更容易在服务间实现“经典式”继承：

	```coffee
  class Human
    constructor: (@name) ->

    talk: (meters) ->
      "I'm talking"

  class Developer extends Human
    code: ->
      "I'm coding"

	app.registerService('Human', Human);
	app.registerService('Developer', Developer);

	```

* 使用 `$cacheFactory` 进行会话级别的缓存，缓存网络请求或复杂运算的结果。

# 模板

* 使用 `ng-bind` 或者 `ng-cloak` 而非简单的 `{{ }}` 以防止页面渲染时的闪烁。
* 避免在模板中使用复杂的表达式。
* 当需要动态设置 <img> 的 `src` 时使用 `ng-src` 而非 `src` 中嵌套 `{{}}` 的模板。
* 当需要动态设置<a>的 `href` 时使用 `ng-href` 而非 `href` 中嵌套 `{{ }}` 的模板。
* 通过 `ng-style` 指令配合对象式参数和 vm 变量来动态设置元素样式，而不是将 scope 变量作为字符串通过 `{{ }}` 用于 `style` 属性。

```html
<script>
...
vm.divStyle = {
  width: 200,
  position: 'relative'
};
...
</script>

<div ng-style="ctrl.divStyle">my beautifully styled div which will work in IE</div>;
```

# 路由

* 在视图展示之前通过 `resolve` 解决依赖。
* 不要在 `resolve` 回调函数中显式使用RESTful调用。将所有请求放在合适的服务中。这样你就可以使用缓存和遵循关注点分离原则。

# 国际化

* 使用 [`angular-translate`](https://github.com/angular-translate/angular-translate)。

# 性能

* 优化 digest cycle

	* 只监听必要的变量。仅在必要时显式调用 `$digest` 循环(例如：在进行实时通讯时，不要在每次接收到消息时触发 `$digest` 循环)。
	* 对于那些只初始化一次并不再改变的内容, 使用一次性 watcher [`bindonce`](https://github.com/Pasvaz/bindonce)。
	* 尽可能使 `$watch` 中的运算简单。在单个 `$watch` 中进行繁杂的运算将使得整个应用变慢(由于JavaScript的单线程特性，`$digest` loop 只能在单一线程进行)
	* 当监听集合时, 如果不是必要的话不要深度监听. 最好使用 `$watchCollection`, 对监听的表达式和之前表达式的值进行浅层的检测.
	* 当没有变量被  `$timeout` 回调函数所影响时，在 `$timeout` 设置第三个参数为 false 来跳过 `$digest` 循环.
	* 当面对超大不太改变的集合, [使用 immutable data structures](http://blog.mgechev.com/2015/03/02/immutability-in-angularjs-immutablejs).


* 用打包、缓存html模板文件到你的主js文件中，减少网络请求, 可以用 [grunt-html2js](https://github.com/karlgoldstein/grunt-html2js) / [gulp-html2js](https://github.com/fraserxu/gulp-html2js). 详见 [这里](http://ng-learn.org/2014/08/Populating_template_cache_with_html2js/) 和 [这里](http://slides.com/yanivefraim-1/real-world-angularjs#/34) 。 在项目有很多小html模板并可以放进主js文件中时（通过minify和gzip压缩），这个办法是很有用的。

# 参考

* [Angular规范](https://github.com/johnpapa/angular-styleguide/blob/master/a1/i18n/zh-CN.md#modules)
* [AngularJS最佳实践和风格指南](https://github.com/mgechev/angularjs-style-guide/blob/master/README-zh-cn.md)
