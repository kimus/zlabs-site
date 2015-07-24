layout: post
title: "Building webfonts with Grunt"
date: 2015-07-24 14:05:12
tags:
- node
- grunt
- webfont
---

![](/images/grunt_webfont.png)

In this article we will try to explain how to use **Grunt** to automate the creation icon fonts from SVG files. I recently used this in a project and I would like to share with you how did I did it.

<!-- more -->

## Requirements

* **Node.JS** and **Grunt**, obviously
* **Grunt-Webfont** Plugin

**Grunt** and **Grunt-Webfont** plugin have their own dependencies. Both have detailed instructions, but I’ll explain the essential steps bellow.


## Create or collect your icons

The first thing to do is preparing the icons grahics, Grunt-Webfont will take SVG or EPS images. You can design your own icons with almost any graphic editor. My personal choice is [Inkscape](https://inkscape.org/). It's multiplatform and it's free, so I’ll strongly suggest you to give a go.

Another option is to pick your icons from existent collections. But please make sure to standardize their sizes and alignments. You can probably get away with just shove the files in the source directory grunt will use.


## Setup Project

The Getting Started Guide on Grunt’s website has everything you need to know to get started, still, to make things simpler, let’s abbreviate it to the indispensable steps. Install the Grunt dependencies and utilities as described on the guide.

### Initialze Node project 
You will need to create a `package.json` file on the root of your project folder, containing something like:

~~~json
{
	“name”: “ZNodeWebfont”,
	“version”: “0.1.0",
	“devDependencies”: {
		“grunt”: “~0.4.1"
	}
}
~~~

Please notice that this file will automatically be updated each time you install or update a Grunt plugin.

Open the project root in Terminal and execute the command `npm install`. This will pull from NPM (Node Package Manager) all the dependencies listed on `package.json`. For nowm, this will install grunt and dependecies.


### The Gruntfile.js File

You will need to create a `Gruntfile.js` (please note the capital G) defining a build script to be used to execute automation tasks and define your plugins configuration.

The basic file structure for the `Grunfile.js` is:

~~~javascript
module.exports = function(grunt) {
	// Project configuration.
	grunt.initConfig({
		pkg: grunt.file.readJSON('package.json'),
		
		// PLUGINS CONFIG
	});

	// Load the plugin that provides the "uglify" task.
	// grunt.loadNpmTasks('pluginname');

	// Default task(s).
	// grunt.registerTask('default', ['taskname']);

};
~~~

### Grunt-Webfont Plugin

Now that we have the base framework in place, let’s install Grunt-Webfont.

Every Grunt plugin usually follows a “convention” for documenting how to install and use it. Grunt-Webfont is no exception and it’s Documentation includes detailed information about the installation process and configuration options.

Note: Grunt-webfont itself it’s based on the Font Custom project, visit the project site to know in more detail what it does. We’re basically automating the process described there.

So, to install Grunt-Webfont please check the documentation on their site for your platform. For Mac we can use Homebrew to install the dependencies by writing in the terminal:

~~~bash
brew install fontforge ttfautohint
~~~

After installing the dependencies we can finally install the Grunt plugin (this will add grunt-webfont to `package.json`):

~~~bash
npm install grunt-webfont -save-dev
~~~


### Add Grunt-Webfont to Grunt file

We now have to instruct Grunt on how we plan to use the plugin.

First will load our plugin by adding at the end to file (look for the similar code in the file we’ve setup before):

~~~javscript
grunt.loadNpmTasks(‘grunt-webfont’);
~~~

Then let’s set our options for building the icon font. The most basic configuration is:
~~~javascript
webfont: {
	icons: {
 		src: ‘icons/*.svg’,
 		dest: ‘build/fonts’
 	}
}
~~~

This will tell Grunt to pick every SVG file on the icons/ folder and create the webfont resources inside the folder build/fonts. It just that simple. You can change the paths as you like.

Let’s see how the `Grunfile.js` should look at this point:

~~~javascript
module.exports = function(grunt) {
	// Project configuration.
 	grunt.initConfig({
 		pkg: grunt.file.readJSON(‘package.json’),

 		webfont: {
 			icons: {
 				src: ‘icons/*.svg’,
 				dest: ‘build/fonts’
 			}
 		}
	});

	// Load the plugin that provides the “uglify” task.
	grunt.loadNpmTasks(‘grunt-webfont’);

	// Default task(s).
	// grunt.registerTask(‘default’, [‘webfont’]);
};
~~~

It’s worth exploring all the available options for this plugin. My final webfont settings is like this:

~~~javascript
webfont: {
	icons: {
		src: 'icons/*.svg',
		dest: 'fonts',
		destCss: 'css',
		htmlDemo: false,
		destHtml: 'fonts',
		options: {
			font: 'zlabs-font',
			templateOptions: {
				baseClass: 'zf',
				classPrefix: 'zf-'
			}
		}
	}
}
~~~

## Build it

If all went well, you’re almost done. In Terminal, at the project root folder, run `grunt webfont`. This will tell Grunt to run that specific task.

~~~bash
grunt webfont
~~~