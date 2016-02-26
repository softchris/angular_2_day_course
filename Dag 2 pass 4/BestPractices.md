# Best practices

## john papas style guide

	https://github.com/johnpapa/angular-styleguide

## One component per file

DO

	user-controller.js
	product-controller.js

DONT

	controllers.js

## project structure
	app
		features
			users
				user.controller.js
				user.tmpl.js
				user.scss
				user.routes.js
			.. next feature ...
		bootstrapping.js
		config.js
		constants.js
		values.js
		app.scss
		specs/
		assets/
		libs/
		dist/
		Gulpfile.js
		karma.conf.js
		

## Watch expressions

	<div ng-repeat="service.getUser()"

	<div ng-model="expression"  

	$scope.prop

	$scope.$watch('scopeprop', function(){

	})



### Keep track of watchers, there is a limit

	https://github.com/pkaminski/digest-hud

	2000 is the limit

## Debugging
Look at scope inspector to see what your scope contains

https://chrome.google.com/webstore/detail/angular-scope-inspector/aaglpchhlnofjbbpopfdfgllfkhnljnl

## Bindings

### ng-bind
When you dont want to display data until its there
	
	ng-bind="prop"
	ng-bind-template="{{prop}} ({{prop2}})"
	
Better than 

	{{prop}} 

### one time binding
Angular defaults to two-binding, this is expensive. Use one-time

	ng-repeat=item in ::items"

	{{ ::prop }}

## ng-if vs ng-show
ng-if removes from DOM, better for memory unless you want to preserve state

GOOD

	<div ng-if="prop">

MIGHT BE BAD, if you want to keep inner state

	<directive ng-if="prop"

## styling
Two supported ways:

	ng-class="a > b ? 'class' : ''"
	ng-class="{ a>b : 'class', b > c : 'class2' }"	
The last version is the only one that is guarenteed to be supported

## Dependency injection
DONT

	angular.module('app').factory('service',function(userService, user){

	});
This will break in minification unless you run ng-annotate

BETTER

	angular.module('app').factory('service',['userService','user',function(userService, user){

	}]);
	
BEST - more readable, this is what minification does anyway

	angular.module('app').factory('service',Service);

	Service.$inject = ['userService','user'];

	function Service(userService, user){

	}

## Watch expression
DONT

	var deepWatch = true;
	$scope.$watch('obj', deepWatch)

THIS IS COSTLY


## dont pollute global namespace


	(function(module) {
		'use strict';
	
		module.controller('appCtrl', AppCtrl);

		function AppCtrl(){

		}
		
	})(angular.module('app'));
		