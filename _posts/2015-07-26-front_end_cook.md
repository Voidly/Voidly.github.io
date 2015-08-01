---
layout: post
title: "前端乱炖"
description: 
modified: 2015-07-26
category: front-end
tags: [front-end]
comments: true
---
##CSS

####BFC(Block formatting context块级格式化上下文)

BFC有一下特性：

* 内部的Box会在垂直方向，从顶部开始一个接一个地放置。
* Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生叠加
* 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
* BFC的区域不会与float box叠加。
* BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。
* 计算BFC的高度时，浮动元素也参与计算。

####BFC触发条件：

* float 除了none以外的值
* overflow 除了visible 以外的值（hidden，auto，scroll ）
* display (table-cell，table-caption，inline-block, flex, inline-flex)
* position值为（absolute，fixed）

**BFC主要用途：作为html布局的一个独立单位，占居相对独立的空间，并不受影响**

##JS

####创建JS对象

如何在Javascript实现OO编程？最好的方式就是充分利用prototype属性。比较基本的原理是，当你用prototype编写一个类后，当你new一个新的object，浏览器会自动把prototype中的内容替你附加在object上。这样，通过利用prototype，你也就实现了类似OO的Javascript。 
{% highlight js %}
function MyObject(name, size)
{
  this.name = name; 
  this.size = size; 
}
MyObject.prototype.tellSize = function()
 {
    return "size of "+this.name+" is "+this.si  
 }
var myobj1 = new MyObject("tiddles", "7.5 meters");
{% endhighlight %}
####闭包
{% highlight js %}
(function(name){
    var say_word = "hello, " + name,
    w = window,
    helloword = function(){
           console.log(say_word);
        };
    w.hw = helloword;
	})("尐鱼");
hw();
{% endhighlight %}

**作用：**

1、封装

2、模块化

####Responsive布局

* media适配：

{% highlight html %}		
<link rel=“stylesheet” type=“text/css” media=“only screen and （max-width： 480px），only screen and （max-device-width： 480px）” href=“link.css”/>
{% endhighlight %}

* 网格流式布局：font-size采用em，宽高等采用%
* 图片自适应

##Angularjs


####视图和模板
{% highlight html %}
<html ng-app>
<head>
 <script src="lib/angular/angular.js"></script> 
 <script src="js/controllers.js"></script> 
</head> 
 <body ng-controller="PhoneListCtrl"> 
 <ul> 
 <li ng-repeat="phone in phones">
   {{phone.name}} <p>{{phone.snippet}}</p> 
 </li> 
 </ul> 
 </body> 
</html>
{% endhighlight %}

####模型和控制器

{% highlight js %}
function PhoneListCtrl($scope) { 
 $scope.phones = 
 [ {"name": "Nexus S", "snippet": "Fast just got faster with Nexus S."}, 
   {"name": "Motorola XOOM™ with Wi-Fi", "snippet": "The Next, Next Generation tablet."},
   {"name": "MOTOROLA XOOM™", "snippet": "The Next, Next Generation tablet."} 
 ]; 
 }
 {% endhighlight %}

####迭代过滤

{% highlight js %}

<div class="container-fluid" ng-controller="PhoneListCtrl"> 
 <div class="row-fluid">  
  <div class="span2"> <!--Sidebar content--> 
 	Search: <input ng-model="query"> 
  </div> 	
 <div class="span10"> <!--Body content-->
	 <ul class="phones"> 
	  <li ng-repeat="phone in phones | filter:query"> 
	  {{phone.name}} <p>{{phone.snippet}}</p> 
	  </li> 
	 </ul> 
  </div> 
 </div> 
</div>
 {% endhighlight %}

####双向绑定

{% highlight js %}
 function PhoneListCtrl($scope) {
 $scope.query = "hello";
}
watch(expression, function(){});
 {% endhighlight %}

####XHR & 依赖注入

{% highlight js %}
function PhoneListCtrl($scope, $http) { 
  	 $http.get('phones/phones.json').success(function(data) { 
  	 $scope.phones = data; 
  	 }); 
 	  $scope.orderProp = 'age'; 
 }
{% endhighlight %}

为了使用AngularJS的服务，你只需要在控制器的构造函数里面作为参数声明出所需服务的名字，就像这样：

{% highlight js %}
  	function PhoneListCtrl($scope, $http) {...}
{% endhighlight %}

当控制器构造的时候，AngularJS的依赖注入器会将这些服务注入到你的控制器中。当然，依赖注入器也会处理所需服务可能存在的任何传递性依赖（一个服务通常会依赖于其他的服务）。

 
####路由

{% highlight js %}
  	angular.module('phonecat', []). 
   	config(['$routeProvider', function($routeProvider) { 
  	 $routeProvider. 
 	 when('/phones', {templateUrl: 'partials/phone-list.html', controller: PhoneListCtrl}). 
	 when('/phones/:phoneId', {templateUrl: 'partials/phone-detail.html', controller: 		PhoneDetailCtrl}). 
	  otherwise({redirectTo: '/phones'}); 
}]);
 {% endhighlight %}

####过滤器

{% highlight js %}
  angular.module('phonecatFilters', []).
  filter('checkmark', function() { 
  return function(input) { return input ? '\u2713' : '\u2718'; 
   	}; 
  });
{% endhighlight %}

{% highlight html %}
 <dl> 
  <dt>Infrared</dt> 
  	<dd>{{phone.connectivity.infrared | checkmark}}</dd> 
  <dt>GPS</dt> 
  <dd>{{phone.connectivity.gps | checkmark}}</dd> 
</dl>
 {% endhighlight %}

####事件监听

{% highlight js %}
 function PhoneDetailCtrl($scope, $routeParams, $http) 
 { $scope.setImage = function(imageUrl) { $scope.mainImageUrl = imageUrl; } }
{% endhighlight %}

{% highlight html %}
 	 <ul class="phone-thumbs"> 
  	 <li ng-repeat="img in phone.images"> 
  	 <img ng-src="{{img}}" ng-click="setImage(img)"> 
  	 </li> 
 	 </ul>
 {% endhighlight %}

####REST和定制服务

{% highlight js %}
  	angular.module('phonecatServices', ['ngResource']). 
	  factory('Phone', function($resource){ 
	   return $resource('phones/:phoneId.json', {}, 
 	  { query: {method:'GET', params:{phoneId:'phones'}, isArray:true} 
 	  }); 
 	 });
	  function PhoneListCtrl($scope, Phone) { 
 	  $scope.phones = Phone.query(); $scope.orderProp = 'age'; 
	  }
{% endhighlight %}

服务有三种方式：

* factory: return {}
* service：this.xxx
* provider: return this.$get(); 使用Provider创建一个service的独特之处是，你可以在Provider对象传递到应用程序的其他部分之前在app.config函数对其进行修改

