
layout: pages
title: angularjs中使用 requirejs 遇到的问题
date: 2015-12-09 00:02:54
categories: [Angularjs]
tags: [Angularjs]

---

## angularjs中使用 requirejs 遇到的问题

### load angular-carousel into my project

- 在 main.js 中：
```javascript
require.config({
    paths: {
        ionic: '../lib/ionic/js/ionic.bundle',
        'ngAnimate': '../lib/angular-animate/angular-animate',
        'ngAria': '../lib/angular-aria/angular-aria',
        'ngMessage': '../lib/angular-messages/angular-messages',
       'ngMaterial': '../lib/angular-material/angular-material',
        'ngCarousel': '../lib/angular-carousel/dist/angular-carousel.min',
        'ngTouch': '../lib/angular-touch/angular-touch.min'
    },
    shim: {
        'ngAnimate':['ionic'],
        'ngAria': ['ionic'],
        'ngMessage': ['ionic'],
        'ngMaterial':{
            deps:['ionic','ngAnimate','ngAria','ngMessage'],
        },
        'ngTouch': {
            deps: ['ionic']
        },
        'ngCarousel':
            [ 'ngTouch']
        
    }

});
require(['app', 'controllers/controllers'], function () {
    angular.bootstrap(document.getElementsByTagName("body")[0], ['app']);//依赖包加载往后手动更新

});
```

- 在app.js 中

```javascript
//ADM格式
define(['app', 'ngCarousel', 'ionic'], function () {
    "use strict";

    return var app = angular.module('app', ['ionic', 'controllers', 'angular-carousel'])
    }); //'angular-carousel'必须和加载的lib中module名一致

```
>requirejs 异步模块定义规范（AMD）
##加载问题解决

* Do not use ng-app in your index.html</p></li>
* Preload all required Angular and Angular Material libraries.

* 
 > 1. Create your own 
 > 2. custom bootstrap code
 > 3. custom main app

 * Do not let RequireJS try to load, require, or define Angular Material components; just set your app dependency on Angular Material  
 
```javascript
angular.module('yourApp',['ng', 'ngMaterial', ... ])>
```
 
