---
layout: post
title:  "Using webpack for react.js applications"
date:   2016-01-23 18:22:29 +0530
categories: articles
---

Let's create a project folder and *package.json* file:

{% highlight sh %}

$mkdir webpack-demo && cd webpack-demo
$npm init

{% endhighlight %}

**Babel**

For this project we will be using babel and it's dependencies which help us transform ES6 files to ES5 javascript.

`$npm install babel-loader babel-core babel-preset-es2015 babel-preset-react --save-dev`


**Webpack**

We will use webpack along with webpack-dev-server which is a handy server that will create bundle and auto reload whenever we make changes in the code.

`$npm install --save-dev webpack webpack-dev-server` 

**Loaders**

We will also require CSS and style loader to include our styles in HTML.

`$npm install --save-dev style-loader css-loader`

**Plugins**

We will also be using *html-webpack-plugin* which will generate HTML file for us.

`$npm install --save-dev html-webpack-plugin`

**React**

`$npm install --save-dev react react-dom`

**Set up webpack and webpack-dev-server**

Add the following lines in the *package.json* file:

{% highlight json %}
	"scripts": {
    "build": "webpack",
    "start": "webpack-dev-server --content-base build"
  }
{% endhighlight %}

So the *package.json* file now looks something like this:

{% highlight json %}
{
  "name": "webpack-demo",
  "version": "1.0.0",
  "description": "Using webpack for react.js applications",
  "main": "index.js",
  "author": "Kushal Dongre",
  "license": "ISC",
  "devDependencies": {
    "babel-core": "^6.4.5",
    "babel-loader": "^6.2.1",
    "babel-preset-es2015": "^6.3.13",
    "babel-preset-react": "^6.3.13",
    "babel-runtime": "^6.3.19",
    "css-loader": "^0.23.1",
    "html-webpack-plugin": "^2.7.1",
    "react": "^0.14.6",
    "react-dom": "^0.14.6",
    "style-loader": "^0.13.0",
    "webpack": "^1.12.12",
    "webpack-dev-server": "^1.14.1"
  },
  "scripts": {
    "build": "webpack",
    "start": "webpack-dev-server --content-base build"
  }
}
{% endhighlight %}

Here you can see there are two scripts: *build* and *start*.

 - The *build* script invokes the webpack and builds a bundle just once.
 - *start* script creates a live reloading server that looks for changes and automatically creates a bundle.

 **Webpack configuration**

 We will now create *webpack.config.js* file which will be in the root directory. This file will contain all the configuration details like input, output, loaders and optionally plugin details. 

{% highlight js %}
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
	entry: {
		app: ['./index.jsx']
	},

	output: {
		path: './dist',
		filename: "bundle.js"
	},

	module: {
		loaders: [
			{
				test: /\.css$/,
				loader: 'style!' + 'css'
			},
			{
				test: /\.(js|jsx)$/,
				exclude: /node_modules/,
				loader: 'babel-loader',
				query: {
					presets: ['react', 'es2015']
				}
			}
		]
	},

	plugins: [
		new HtmlWebpackPlugin({
			title: 'This is a react app which uses webpack'
		})
	]
};

{% endhighlight %}

- The configuration file contains an entry point. In our example it is *index.jsx*. 
  Lets create the file `$touch index.jsx`

{% highlight js %}
import './style.css';
import React from 'react';
import ReactDom from 'react-dom';

ReactDom.render(<h1>Hello from react backed by webpack</h1>,
document.body);
{% endhighlight %}

- Next we have specified output file *bundle.js* which will be under the folder *dist*
- *Loaders*: It contains *test* which has a regex. In our example it specifies css files. The *loader* key contains the npm module names chained in reverse order. In our example we want to take css file and load it has style tag. The chaning is done by specifying '!' mark.
- *Plugins*:
	We are using *html-webpack-plugin* which will create HTML file for us with a title specified in config file.

This config file allows us to build bundle in two ways:

**build**

`$npm run build` which runs only once. 

{% highlight sh %}
Hash: fe111a361b4ba2fb4cd8
Version: webpack 1.12.12
Time: 549ms
     Asset       Size  Chunks             Chunk Names
 bundle.js     1.5 kB       0  [emitted]  app
index.html  186 bytes          [emitted]
   [0] multi app 28 bytes {0} [built]
   [1] ./index.jsx 0 bytes {0} [built]
Child html-webpack-plugin for "index.html":
        + 3 hidden modules
{%endhighlight%}

**start**

`$npm run start` runs server on port 8080

{% highlight sh %}
http://localhost:8080/webpack-dev-server/
webpack result is served from /
content is served from /Users/kushal/workspace/reactjs/demo/webpack-demo/build
Hash: fe111a361b4ba2fb4cd8
Version: webpack 1.12.12
Time: 541ms
     Asset       Size  Chunks             Chunk Names
 bundle.js     1.5 kB       0  [emitted]  app
index.html  186 bytes          [emitted]
chunk    {0} bundle.js (app) 28 bytes [rendered]
    [0] multi app 28 bytes {0} [built]
    [1] ./index.jsx 0 bytes {0} [built]
Child html-webpack-plugin for "index.html":
    chunk    {0} index.html 412 kB [rendered]
        [0] ./~/html-webpack-plugin/lib/loader.js!./~/html-webpack-plugin/default_index.ejs 346 bytes {0} [built]
        [1] ./~/lodash/index.js 411 kB {0} [built]
        [2] (webpack)/buildin/module.js 251 bytes {0} [built]
webpack: bundle is now VALID.
{%endhighlight%}

You can now open the location [http://localhost:8080/webpack-dev-server/](http://localhost:8080/webpack-dev-server/) in browser and see HTML page with our title showing up. 

[Source code](https://github.com/kushald/webpack-demo)

That's it!



