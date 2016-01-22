---
layout: post
title:  "Transform ES6 code to ES5 using babel"
date:   2016-01-22 17:48:29 +0530
categories: article
---

Prerequisites
=================

- [Node.js][Node.js]
- [Npm][Npm]

Lets first create a folder where our code will reside.

`$mkdir babel-demo && cd babel-demo`

Create package.json file which will be used to install all of our dependencies.

`$npm init`

{% highlight sh %}
name: (babel-demo)
version: (1.0.0)
description: Setup app to run ES6 on browser
entry point: (index.js)
test command:
git repository:
keywords:
author:
license: (ISC)
About to write to /Users/kushal/workspace/babel/babel-demo/package.json:

{
  "name": "babel-demo",
  "version": "1.0.0",
  "description": "Setup app to run ES6 on browser",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this ok? (yes)

{% endhighlight %}


Next we would require following packages:

**Babel**

{% highlight sh %}
$npm install --save-dev babel
$npm install --save-dev babel-preset-es2015
$npm install --save-dev babelify
{% endhighlight %}

These packages will help transform ES6 code to ES5.

**Browserify**

`$npm install --save-dev browserify`

Browserify is basically used for writing code in node.js style. It makes code isomorphic.

**Gulp**

`$npm install --save-dev gulp`

Gulp is a build tool to run tasks. We will need it to bundle javascript file and copy it in destination folder.

**vinyl-source-stream**

`$npm install --save-dev vinyl-source-stream`

Gulp by default does not work with modules like browserify. vinyl-source-stream allows us to use these modules in Gulp task.

After all the dependencies are installed, your package.json file should look something like this:

{% highlight json %}
{
  "name": "babel-demo",
  "version": "1.0.0",
  "description": "Setup app to run ES6 on browser",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel": "^6.3.26",
    "babel-preset-es2015": "^6.3.13",
    "babelify": "^7.2.0",
    "browserify": "^13.0.0",
    "gulp": "^3.9.0",
    "vinyl-source-stream": "^1.1.0"
  }
}
{%endhighlight%}



The updated *package.json* will now contain all the packages mentioned in devDependencies along with version.

**gulpfile.js**

Next create gulfile.js in project directory which will contain tasks we want to run.

{% highlight js %}
'use strict';

var gulp = require('gulp');
var babelify = require('babelify');
var browserify = require('browserify');
var src = require('vinyl-source-stream');

// Build task 
gulp.task('build', function() {
	browserify('index.js')
	.transform('babelify', {presets: ["es2015"]}) //transform ES6 to ES5 using babelify
	.bundle() // bundle the transformed file into a single file
	.pipe(src('bundle.js')) // pipe it to bundle.js file
	.pipe(gulp.dest('./js')); // copy bundle file in js folder
});

{%endhighlight%}

To test this we will create *index.js* in project directory which will contain ES6 code.

{% highlight js %}

//index.js

class Person {
	constructor(name, age) {
		this.name = name;
		this.age = age;
	}
}
{%endhighlight%}

Finally we will run build task to transform the above code to ES5.

{% highlight sh %}
$gulp build
[21:55:01] Using gulpfile ~/workspace/babel/babel-demo/gulpfile.js
[21:55:01] Starting 'build'...
[21:55:01] Finished 'build' after 22 ms
{%endhighlight%}



Gulp task will create bundle.js file in js folder with ES5 code in it.

{% highlight js %}
(function e(t,n,r){function s(o,u){if(!n[o]){if(!t[o]){var a=typeof require=="function"&&require;if(!u&&a)return a(o,!0);if(i)return i(o,!0);var f=new Error("Cannot find module '"+o+"'");throw f.code="MODULE_NOT_FOUND",f}var l=n[o]={exports:{}};t[o][0].call(l.exports,function(e){var n=t[o][1][e];return s(n?n:e)},l,l.exports,e,t,n,r)}return n[o].exports}var i=typeof require=="function"&&require;for(var o=0;o<r.length;o++)s(r[o]);return s})({1:[function(require,module,exports){
"use strict";

function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

var Person = function Person(name, age) {
	_classCallCheck(this, Person);

	this.name = name;
	this.age = age;
};

},{}]},{},[1]);
{%endhighlight%}

###[Source code][source-code]

That's it!

[Node.js]: https://nodejs.org/
[Npm]: https://www.npmjs.com/
[source-code]: https://github.com/kushald/babel-transform-es6-es5