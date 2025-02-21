---
title: "Introduction to Gulp.js 2 – Server with BrowserSync"
author: Stefan Imhoff
date: 2014-10-19T10:17:00+02:00
description: "The ultimative tutorial and guide for Gulp.js: How to set up a development server with BrowserSync."
tags:
  - code
series:
  - gulp
---

This is the 2nd part of my series _Introduction to Gulp.js_. Today I will write the first few Gulp.js tasks and set up a development server with BrowserSync. And I will start to write a configuration file.

## Installing Gulp.js

To run my `gulpfile.js` I need to install gulp:

```bash
$ npm install --save-dev gulp@3.9.0
```

If I run the command `gulp` on my command line I get an error message <samp>Task 'default' is not in your gulpfile</samp>. This is because I haven’t written a gulp task until now.

I create inside the `gulp/tasks` folder a file `default.js` and write this code:

#### gulp/tasks/default.js

```javascript
var gulp = require("gulp");

gulp.task("default", function () {
  console.log("Hello Gulp.js!");
});
```

I know … I said I’m sick of _Hello World_ tutorials, but this won’t last very long. I’ll soon replace it with some valuable code. So stay with me.

If you execute the command `gulp`, this Gulp.js task will output <samp>Hello Gulp.js!</samp> to the console.

I will speed up the pace a little bit from now on.

## Watch

Instead of calling a function and output some text to the console, I can execute tasks. I decided to execute the watch task when running `gulp`. This task will later watch for changes in files and update my files.

#### gulp/tasks/default.js

```javascript
var gulp = require("gulp");

gulp.task("default", ["watch"]);
```

It’s possible to run multiple tasks at once, which is why I write my `watch` task in an Array. Be careful: These tasks will run in parallel, not in sequential order. Later I will show how to run tasks in a predefined order.

I will create another folder within my `tasks` folder with the name `development` and put all tasks needed for development in this folder. This is not necessary, but I did so:

#### gulp/tasks/development/watch.js

```javascript
var gulp = require("gulp");

/**
 * Start browser-sync task and then watch files for changes
 */
gulp.task("watch", ["browsersync"], function () {});
```

I will come back later to write the `watch` task. For now, the function will be empty and just run another task before running the watch task: `browser-sync`. All tasks within the Array will be executed _before_ the task is executed.

## BrowserSync

You might have heard of [LiveReload](http://livereload.com/), a tool that is watching for changes in your files and automatically reloads the server. With Stylesheets, even reloading is not needed. The page refreshes with the changes instantly.

But [BrowserSync](https://browsersync.io/) is even better: It does all LiveReload does, but you don’t need a browser plugin and it syncs your actions like a scroll, click, refresh or filling out forms to all browsers connected. This works even with mobile devices. And BrowserSync has even support for a development server. That’s why I will need nothing more than BrowserSync to get a development server with live reloading.

But first I install BrowserSync:

```bash
$ npm install --save-dev browser-sync@2.9.11
```

I create a new file `browser-sync.js` in `gulp/tasks/development/`. This file will start BrowserSync and the development server.

#### gulp/tasks/development/browser-sync.js

```javascript
var gulp = require("gulp");
var browsersync = require("browser-sync");
var config = require("../../config").browsersync.development;

/**
 * Run the build task and start a server with BrowserSync
 */
gulp.task("browsersync", ["build"], function () {
  browsersync(config);
});
```

This code does need some explanation: First I load Gulp.js and BrowserSync, which are needed in this task. Then I load the configuration for BrowserSync. I will create this configuration file in a moment. Keeping all configuration out of the tasks will make them more usable and they can be easily shared between different projects.

The second thing worth mentioning is `['build']`. This does mean before starting BrowserSync it first will run the `build` Gulp.js task (which I will write later). Every Gulp.js task needs a name. As a second parameter, you can either add a JavaScript callback or tasks or both.

## Configuration

I create a new file `config.js` in the main Gulp.js folder:

#### gulp/config.js

```javascript
var src = "app";
var build = "build";
var development = "build/development";
var production = "build/production";
var srcAssets = "app/_assets";
var developmentAssets = "build/assets";
var productionAssets = "build/production/assets";

module.exports = {
  browsersync: {
    development: {
      server: {
        baseDir: [development, build, src],
      },
      port: 9999,
      files: [
        developmentAssets + "/css/*.css",
        developmentAssets + "/js/*.js",
        developmentAssets + "/images/**",
        developmentAssets + "/fonts/*",
      ],
    },
  },
};
```

First I extract some paths needed over and over again later to variables and then I create a CommonJS module and add an entry for BrowserSync. BrowserSync runs with default options, but I want to override the port and I tell BrowserSync which folders should be served.

Jekyll wipes out all files on recreation and to speed up development I have to be creative because I don’t want to recreate all assets on every Jekyll build. That’s why I serve more than one folder. I serve the folder `build/development`, which will hold the files created by Jekyll. The assets I will generate into a different folder `build/assets` so Jekyll doesn’t wiped them out. And additionally the folder `app/_assets` to link source maps later.

BrowserSync watches only my asset files, in order that my browser won’t reload like hell, every time Jekyll creates one file. I will later write one task, which reloads the Browser one time after the Jekyll build is complete.

## Conclusion

This concludes the 2nd part of my series _Introduction to Gulp.js_. We learned how to install Gulp.js, how to write a Gulp.js task, run other tasks, and set up a development server with BrowserSync.

{% more "View Source on GitHub", "https://github.com/kogakure/gulp-tutorial", true %}
