layout: pages 
title: angular笔记 
date: 2016-03-18 14:52:55
categories: [Angularjs]
tags: [Angularjs]
---

# this and $scope
$scope 是全局的，可以直接使用

```javascript
function ACtrl($scope) {
                   $scope.test = "一个例子"; //在$scope对象中加入test
               }
```
html:
``` html
<div ng-controller="ACtrl">
                    {{test}}
                </div>
```

this 则只能在当前controllers中使用
```javascript
function ACtrl() {
              var vm = this ;   
              this.test = "一个例子"; //在$scope对象中加入test
               }
```

html:
```html
<div ng-controller="ACtrl as a">
                    {{a.test}}
                </div>
```
