layout: post
title: "How to create a WebApp project with Node.js"
date: 2015-05-03 13:11:26
tags:
- node
---

![](/images/node_webapp.png)

In this article, we'll take a look at how you can usin Node.js in conjunction with Express to speed up your development cycle.

We'll start with some Node.js basics, including the initial configuring steps with npm and build a simple web server for development.

<!-- more -->

## Getting Started with Node.js

### What is Node.js?

**Node.js** is a platform built on **Chrome's JavaScript runtime** for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

### npm utility

**Node.js** comes bundled with **npm** utility that is the official package manager for Node.js, and provides a command line interface (CLI) for interacting with the online registry for open-source Node.js projects, modules, resources, etc. You can find it at http://npmjs.org.

For API documentation, visit https://npmjs.org/doc/ or just type npm in your terminal.


### Installing node.js and npm

First of all we need to install **Node.js** and **npm** in order to follow along. Try one of the following install options:

* Ubuntu/Debian user can run `apt-get install nodejs`
* Mac/Homebrew users can run `brew install node`
* Windows users or alternative can download a binary from http://nodejs.org/
* Use Node Version Manager (NVM)


### Initial configuration

After installing we can, optionally, configure **npm** a little bit. Go ahead and enter these commands in a terminal, using your own information. This way, when we run some npm commands later, it will already know who we are and will be able to autocomplete some information for us.

~~~bash
npm set init.author.name "John Doe"
npm set init.author.email "john.doe@gmail.com"
npm set init.author.url "http://johndoe.com"
~~~

If you wan't to publish node modules to the online registry you will need to create a user. This next command will prompt you for an email and password, create or verify a user in the npm registry, and save the credentials to the ~/.npmrc file.

~~~bash
npm adduser
~~~

## Create a Node.js module

A **Node.js/npm** module is just an ordinary JavaScript file with the addition that it must follow the CommonJS module spec. This is really not as complex as it sounds. Node modules run in their own scope so that they do not conflict with other modules. Node relatedly provides access to some globals to help facilitate module interoperability. The primary 2 items that we are concerned with here are require and exports. You require other modules that you wish to use in your code and your module exports anything that should be exposed publicly. For example:

~~~javascript
var other = require('other_module');
module.exports = function() {
    console.log(other.doSomething());
}
~~~

To get started, let's initialize our new module by executing the `init` command from **npm** utility. This command will ask you a bunch of questions, and then write out a `package.json` file. It is this file that effectively turns your code into a package.

~~~bash
npm init
~~~

Have a look to see what the file contains; it is pretty human-readable. Further details and explanation of the contents of the package.json file can be found at https://npmjs.org/doc/json.html. Our initial version looks like the following, but we’ll be updating this further as we go along.

~~~json
{
  "name": "ZWebNode",
  "version": "1.0.0",
  "description": "A node Web application example",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Helder Rossa <kimus.linuxus@gmail.com>"
}
~~~

### Main script file

The main script file is generally the starting point of a **Node.js** module. By default the name of the script is `index.js`. In this example we have name it to `server.js` because the file will start our development testing server. Later in the article we will implement this.

### Module Requirements

In this examples we will require some modules for creating a web server to serve our files. But there's other modules and you can even create your own web server without the need of module dependencies.

So, for our example we will use **Express**. Let's use it:

~~~bash
npm install --save-dev express
~~~

The above commands will also create a `node_modules` folder in your project directory containing, in this case, **Express** dependencies. Following best practices, we’ll want to keep the `node_modules` folder out of the VCS repositories.

The `--save-dev` switch tells **npm** to save dependency in the `package.json`.

~~~json
  "devDependencies": {
    "express": "^4.12.3"
  }
~~~

### Using Express for Serving files 

Now we can actually get on to the business of writing some code. Create an `server.js` file to hold the web server code. It’ll look something like the following.

~~~javascript
var express = require('express'),
	http = require('http'),
	path = require('path'),
	app = express();

app.set('port', process.env.PORT || 9080);

// static files serving
app.use('/', express.static(path.join(__dirname, '/static')));

var server = http.createServer(app);

server.listen(app.get('port'), function()
{
	console.log('server listening on port ' + app.get('port'));
	console.log('>>> http://localhost:' + app.get('port') + '/');
});
~~~

And that's it! Create a directory named `static`and put your web static contents (html, css, js, etc) there and start coding.


### Readme File

It is always a good idea to include some documentation with your project, so I’ll add a `README.md`, using markdown syntax. Using markdown is a good idea because it will be nicely displayed on both Github and npm.

~~~markdown
ZWebNode
========

A Web application project using Node.js.

## Release History

* 1.0.0 Initial release
~~~

Great, the project is complete.


