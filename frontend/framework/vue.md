# 简介

本风格指南的目的是展示[VueJS](https://github.com/vuejs/vue)在OmniSocials应用的最佳实践和风格指南，ES6语言风格指南[参考](https://github.com/frontnode/style-guide/blob/master/frontend/language/es6.md)

# 内容目录
* [概览](#概览)
    * [目录结构](#目录结构)
    * [标记](#标记)
    * [命名约定](#命名约定)
    * [其他](#其他)
* [模块](#模块)
* [页面](#页面)
* [组件](#组件)
* [样式](#样式)
* [构建工具](#构建工具)

# 概览

## 目录结构

由于一个大型的VueJS应用有较多组成部分，所以我们是通过拆分模块的方式来组织的。整个项目拆分为公共组件部分和业务模块部分，公共组件部分就是在h5目录下面的所有除`modules`目录以外的部分，所有的业务模块都放在`modules`目录下面

* 总体的vue手机页面前端代码结构如下：

```
static/h5
    ├── components
    ├── index.ejs
    ├── modules
    ├── pay.ejs
    ├── README.md
    ├── styles
    │   ├── mixins.less
    │   ├── reset.less
    │   ├── util.less
    │   └── variables.less
    └── utils
        ├── address.js
        ├── interaction.js
        └── sessionStorage.js
```

**说明：**
    * `components`是跨模块的公共组件，业务模块可以根据自已的需要引入。
    * `index.ejs`是应用的公共入口页面模板，不需要业务模块自行添加和修改。
    * `modules`是应用下的所有业务模块。
    * `pay.ejs`是应用的支付入口页面模板，不需要业务模块自行添加和修改。    
    * `README.md`是对项目的基本介绍。
    * `styles`是跨模块的公共样式，业务模块可以根据自已的需要引入。
    * `utils`是跨模块的公共JS模块，业务模块可以根据自已的需要引入。  

* 业务模块按照业务优先的组织方式目录结构看起来如下：

```
static/h5/modules/module
    ├── App.vue
    ├── components
    │   ├── DatePicker.vue
    │   └── images
    ├── images
    ├── locales.js
    ├── main.js
    ├── pages
    │   ├── activate
    |   ├── ...
    │   └── profile
    ├── routers.js
    └── styles
        ├── mixin.less
        ├── reset.less
        ├── util.less
        └── variables.less
```

**说明：**
    * `App.vue`是应用的入口组件（定义了路由，页面需要的less文件也在这里引入）。
    * `components`是模块下的公共组件，当组件被其他业务模块引用可考虑提升为跨模块公共组件。
    * `images`是模块下的使用的图片资源。
    * `locales.js`是页面国际化引用的定义。
    * `main.js` 是应用的入口文件。
    * `pages` 是模块下的所有业务页面。
    * `routers.js`是应用的页面路由定义。
    * `styles`是模块下的公共样式，当组件被其他业务模块引用可考虑提升为跨模块公共样式。    
    * 当目录里有多个单词时, 使用 lisp-case 语法: `static/h5/modules/my-complex-module/`。

* 业务模块下业务页面的目录结构看起来如下：

```
static/h5/modules/module/pages/page
    ├── page.less
    ├── page.vue
    └── i18n
        ├── locate-en_us.js
        └── locate-zh_cn.js
```

**说明：**
    * `page.less`是业务页面的样式文件。
    * `page.vue`是业务页面的vue文件。
    * `i18n`是放置业务页面国际化文件的目录。

## 标记

保持标签的简洁并把VueJS的标签放在标准HTML属性后面，可选的VueJS属性放在必填的VueJS属性之后。这样提高了代码可读性。标准HTML属性和VueJS的属性没有混到一起，提高了代码的可维护性。

```html
<section class="list-wrapper" v-if="items.length">
  <div class="radio-list" v-for="item in items" track-by="$index" v-tap="selectCoupon($index)">
      <radio class="radio" :name="item.name" :text="item.label"></radio>
  </div>
</section>
```

其它的HTML标签应该遵循下面的指南的 [建议](http://mdo.github.io/code-guide/#html-attribute-order)

## 命名约定

TODO

## 其他

* 使用[Vue Resource](https://github.com/vuejs/vue-async-data)加载页面的初始数据，通过回调中的resolve函数将数据更新到视图上（resolve可以被执行多次）。
* 在`main.js`中设置请求的root path，使用`$resource`方法发送请求（请求地址前面不带`/`）。
* 开发中优先使用const，使用const修饰的字面量常量需要大写，其他初始化之后不需要再更新的值使用驼峰表示。
* 尽量使用简写的形式使用指令，例如v-bind:href="url"统一简写成:href="url"; v-on:focus="dosomething"简写成@focus="dosomething"。

# 模块

* 扩展模块下的手机端相关代码放在模块的`h5`目录下面，核心模块下的手机端相关代码放在`static/h5/moduleName`下（moduleName需要替换为真实模块名称），遵循标准的手机业务模块目录结构。
* 手机业务模块下页面使用的图片放在`images`文件夹下面。
* 模块中主要的入口文件只有一个`main.js`，只有模块下需要支付页面的时候添加支付页面入口文件`pay.js`。
* 模块下的页面使用了延迟加载的机制，需要遵循页面的开发说明增加新的页面。

## 添加手机页面支付支持

* 在模块的`webapp`下的`controllers`文件夹中增加moduleNameContorller（moduleName需要替换为真实模块名称）,模板文件如下：

```php
<?php
namespace webapp\modules\moduleName\controllers;

use Yii;
use yii\web\Controller;

class moduleNameContorller extends Controller
{
    public $layout = 'main';

    /**
     * Render the order of moduleName mobile pages
     */
    public function actionPay()
    {
        return $this->render('pay');
    }
}
```

* 在模块的`webapp`下的`views`文件夹中增加`layouts`文件夹，并在`module`文件夹（module需要替换为真实模块名称）下增加`pay.php`文件,模板文件如下：

```php
<app></app>
```

* 所有的样式引入在`pay.vue`中完成, `pay.js`的模板文件如下:

```js
import Vue from 'vue'
import VueResource from 'vue-resource'
import VueAsyncData from 'vue-async-data'
import VueTap from 'vue-tap'
import VueI18n from 'vue-i18n'
import App from './pages/pay/pay.vue'

Vue.use(VueResource)
Vue.use(VueAsyncData)
Vue.use(VueTap)
Vue.use(VueI18n, {
  lang: 'zh_cn',
  locales: {
    'en_us': {
      pay: require('./pages/pay/i18n/locate-en_us')
    },
    'zh_cn': {
      pay: require('./pages/pay/i18n/locate-zh_cn')
    }
  }
})

Vue.http.options.root = '/api'

// Define your root component for app here
new Vue({
  el: 'body',
  components: { App }
})
```

* 支付页面跟正常页面一样放在`pages`目录下维护，使用`pay`命名对应文件夹。

# 页面

* 模块下的手机页面放在手机模块的`pages`目录下面，遵循业务模块下业务页面的目录结构。
* 在页面的template标签下根节点需要加上页面的名字作为id，这个id在页面的less文件中作为namespace使用。
* 页面中不允许使用`style`标签。
* 模板插值语法全部用v-text，而不用{{}}，国际化的写法使用`v-text="$t(item.brief)"`。
* 页面的视图模型data里面的数据应该只和视图渲染相关，不要直接把获取的数据赋值到vm上，methods里面应该只放和页面交互相关的操作。
* 在data方法中全局变量绑在this上，例如`this.query`表示查询参数，`this.pagination`表示分页参数, `this.params`表示地址path中的参数。
* 单纯的数据映射逻辑不要写在watch里，而是写在computed里。

## 标准的页面模板(Vue文件)

```vue
<template>
  <div id="page">
    ...
  </div>
</template>

<script>
import Component from '../../components/Component'

...

export default {
  data () {
    return {
      ...      
    }
  },
  methods: {
    ...
  },
  ready () {
    ...
  },
  components: {
    Component
  }
}
</script>
```

## 标准的页面JS逻辑模板(script部分)

```js
属性的换行规则
生命周期函数的顺序
页面跳转，query参数加注释，query不变，直接return，加新的参数merge，减少参数直接删
```

# 组件

* 使用大写字母开头的驼峰法命名指令。
* 组件中可以直接使用`style`标签来存放组件的样式，但是需要加上`scoped`属性。

## 标准的组件模板(Vue文件)

```vue
<template>
  <div id="component-name">
    ...
  </div>
</template>

<style scoped lang='less'>
@import '../../../../static/h5/styles/mixins.less';
@import '../styles/variables';
@import '../styles/mixin';

#component-name {
}
</style>

<script>
import DependedComponent from './DependedComponent'

export default {
  props: {
    description: { // description of goods
      type: String,
      default: ''
    },
    number: { // the number of current goods in cart
      type: Number,
      required: true,
      twoWay: true
    }
    ...
  },
  computed: {
    ...
  },
  components: {
    DependedComponent
  }
}
</script>
```

# 样式

* 多页面复用的全局变量放在`styles/variables`文件下，组件特有的变量定义在`style`标签中，页面中特有的变量定义在页面less文件里。
* TODO:　使用px2rem的mixin定义px的值。

# 参考

* [VueJS中文官网](http://cn.vuejs.org/)
