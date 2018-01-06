---
title: Grunt
media_order: grunt-logo.png
date: '13-11-2015 17:40'
taxonomy:
    tag:
        - JavaScript
        - 'Example project'
        - Grunt
header_image_file: grunt-logo.png
---

If you work on a larger project, you have surely write a build script, which helps you with repeating tasks. This can be concatenating javascripts, compiling SASS / LESS files, moving or deleting some files, etc. Each task like this can be ( relatively ) easily defined in Grunt and it will take care of everything.

===

#### Example

No doubt that you worked on some project where you spread CSS and JavaScript things into several files to have order and overview. But than a problem occurs. You have to add individual scripts one by one into html header. Then it looks like this :

```
<head>
    ...
    <link href="file1.css" rel="stylesheet">
    <link href="file2.css" rel="stylesheet">
    <link href="file3.css" rel="stylesheet">
    
    <script src="file1.js"></script>
    <script src="file2.js"></script>
    ...
</head>
```

A simple solution can be manually and laboriously merging files, what is boring and you have to do it after any change. Or you can use a **task manager**, which would take care of everything. Of course there are many task managers. The most known among all are **Grunt** and **Gulp**.

#### What is it capable to do?

One can say whatever is necessary. From merging files, minifying, moving to testing functions and tracking file changes. There are lot of available modules like this and in case there is none for what is needed, it is not hard to create your own.

#### Installation

Grunt and its plugins are installed via **[NPM](https://www.npmjs.com/)** package manager for **[Node.js](https://nodejs.org/)** :

```
$ npm install -g grunt-cli
```

Parameter `-g` installs package `grunt-cli` globally and you may need to use <kbd>sudo</kbd> to successfully run the installation, or in Windows run command shell as Administrator.

**Beware!** This is some kind of launcher for Grunt. It, itself, is located in project folder next to the configuration file Gruntfile along with other dependencies.

## Project *GruntTest* as example

The easiest and most appropriate way is to show it on example. No need to start anything complicated and big for beginning. Let’s have a project which will have two main JavaScript files. One is *maths.js*, where are functions `sum`, `subtract` and second is *text.js* with functions `convertToUpperCase` and `convertToLowerCase`. On the page, everything should be in one file, without any unnecessary characters or comments. To see, which files are intended for development and which for production, the directory tree will look like this :

```
src ( source code )
    maths.js
    text.js
www ( clean code for production )
    index.html
    js
        grunttest.min.js ( final javascript )
```

#### File *package.json*, informations about the project and dependencies

File *package.json* belongs in the project root directory and it holds project metadata and dependencies. You can create it using NPM – with command <kbd>npm init</kbd>. It asks for project name, version, author, etc. and accordingly generates basic *package.json* file.

```
{
  "name": "GruntTest",
  "version": "1.0.0",
  "description": "Test project",
  "main": "",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Matej 'juffalow' Jellus",
  "license": "ISC"
}
```

This would be a basic file without any dependencies. But the task description says there should be only one, minimized file. I.e. you need to find such a plugin that will merge files into one and another that will erase everything superfluous. List of these plugins could be found on official webpage [Grunt Plugins](http://gruntjs.com/plugins).

After a short look at the basic plugin offer, it is clear that *contrib-concat* with definition “concatenate files” and *contrib-uglify* with definition “minify files” are meeting the requirements. Thus these two plugins add as dependency into package.json. Now it looks like this :

```
{
  "name": "GruntTest",
  "version": "1.0.0",
  "description": "Test project",
  "author": "Matej 'juffalow' Jellus",
  "devDependencies": {
    "grunt": "~0.4.5",
    "grunt-contrib-concat": "~0.5.1",
    "grunt-contrib-uglify": "~0.5.0"
  }
}
```

#### Installing Grunt and plugins

Each project can use different version of Grunt and different plugins. If you want to install all dependencies at once, just run command :

```
$ npm install
```

It will download all needed packages and saves them in subdirectory *node_modules* in project directory. Among them is also Grunt itself, which is executed from the console with the command <kbd>grunt</kbd>.

#### File *Gruntfile.js* and task creating

This file belongs to the project root directory either. There are defined and configured tasks to be executed by Grunt. It consist of 4 main parts :

* main function – wraps whole functionality, i.e. whole Grunt code must be defined inside it
* configuration – all plugins require at least basic settings, for example if they are to merge files, there has to be defined a directory where the files are and path to the output file
* load plugins – plugins, which are used must be loaded first ( something like *include* )
* tasks – task names and plugins to them, which have to be run

*Gruntfile.js*, which will solve the problem could be written as follows :

```
// main function
module.exports = function(grunt) {

  // configuration
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    concat: { // plugin, which is concatenating files
      options: {
        separator: ';'
      },
      dist: {
        src: ['src/*.js'], // the input are all .js files in src directory
        dest: 'src/temp/<%= pkg.name %>.js' // output is one file named like the project ( defined in file package.json -> name )
      }
    },
    uglify: { // plugin, which minify files
      options: {
        banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
      },
      build: {
        src: '<%= concat.dist.dest %>', // the input is the output file from concat plugin
        dest: 'www/js/<%= pkg.name %>.min.js' // output is minify file in production directory
      }
    }
  });

  // load plugins
  grunt.loadNpmTasks('grunt-contrib-concat');
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // default task
  // plugins are executed in the order they are defined here
  grunt.registerTask('default', ['concat', 'uglify']);

};
```

When everything is prepared and ready, you only have to run the command <kbd>grunt</kbd>, which if is typed without parameter it does only default task – in this case the first step is to concatenate all .js files from directory src into one file GruntTest.js in directory */src/temp*. After that the minify plugin is run. It will get the output file from concatenation as input ( **caution!** because it is defined this way, the concat plugin must be executed before minimization! ). It deletes all redundant white characters + comments and creates ugly / hard to read single line code. This output is then saved into *www/js/* directory.

## Cleaning up the project

An extra file was created when concatenating javascripts. As the project grows, there can occur more such files, or simply files to be deleted from time to time. You can create another task for this and its job would be removing these files.

#### Adding a new plugin

One of the ways to add new plugin is to write it into package.json as a new dependency and run command <kbd>npm install</kbd>. Alternatively, or maybe simpler, way is to run command for installing particular plugin with parameter `--save-dev`, which downloads the plugin and also write it into *package.json* itself.

```
$ npm install <module> --save-dev
```

So back to [grunt plugins](http://gruntjs.com/plugins) page, where you have to find something what erases files / directories. The *contrib-clean* with description “clean files and folders” should manage it.

```
$ npm install grunt-contrib-clean --save-dev
```

#### Creating new task

Plugin added, inscribed in dependencies, leaves only configuration and creating the task in *Gruntfile.js*. That is not anything hard, basically three lines in addition :

```
// configuration
...
clean: ["src/temp/"]
...
```

```
// load plugins
...
grunt.loadNpmTasks('grunt-contrib-clean');
...
```

```
// new task
...
grunt.registerTask('cleanup', ['clean']);
...
```

#### Executing your own tasks

You can try it with running <kbd>grunt cleanup</kbd>. Parameter is the name of the task, which has been created. If Grunt is executed without any parameter, it will perform only the default task.

## Conclusion

This should be all. I can surely recommend to see the list of another plugins. Actually I am using *grunt-remove-logging*, which removes every `console.log()`, *grunt-contrib-jshint* for validating JavaScripts ( see [jshint.com](http://jshint.com/) ). You can use Grunt for CSS files, compiling SASS and a lot of other things.

The whole project is available to see or download on [GitHub](https://github.com/juffalow/GruntTest).

#### References

[Official Grunt web page](http://gruntjs.com/) where you can find introduction tutorial, list of plugins and whole documentation.
Plugins and their documentation is on [NPM package manager](https://www.npmjs.com/) website. Links on used plugins :

* [grunt-contrib-concat](https://www.npmjs.com/package/grunt-contrib-concat) – merging files
* [grunt-contrib-uglify](https://www.npmjs.com/package/grunt-contrib-uglify) – minifying files
* [grunt-contrib-clean](https://www.npmjs.com/package/grunt-contrib-clean) – deleting files and directories
