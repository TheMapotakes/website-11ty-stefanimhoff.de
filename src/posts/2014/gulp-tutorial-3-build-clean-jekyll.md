---
title: "Introduction to Gulp.js 3 – Build, Clean and Jekyll"
author: Stefan Imhoff
date: 2014-10-20T10:00:00+02:00
description: "The ultimative tutorial and guide for Gulp.js: How to write tasks for cleaning files and folders, generating the build and the website with Jekyll."
tags:
  - code
series:
  - gulp
---

This is the 3rd part of my series _Introduction to Gulp.js_. Today I will write the build task, which will execute all other tasks needed for a build, the task to delete assets for a fresh start, and the task to create my Jekyll site.

## Build

Now I create a `build` task. This task will run all other tasks, which are needed to create the site. By default, Gulp.js runs all tasks in parallel. That’s why I will get a problem if a specific order is needed. I will need a node module which runs tasks in a sequence:

```bash
$ npm install --save-dev run-sequence@1.1.4
```

Next I create the task:

#### gulp/tasks/development/build.js

```javascript
var gulp = require("gulp");
var runSequence = require("run-sequence");

/**
 * Run all tasks needed for a build in a defined order
 */
gulp.task("build", function (callback) {
  runSequence(
    "delete",
    ["jekyll", "sass", "scripts", "images", "copy:fonts"],
    "base64",
    callback
  );
});
```

This task will first delete the assets folder (Jekyll is deleted by default), then create in parallel the Jekyll site, CSS files from SASS files, bundle the JavaScript files, copy images to the assets folder and copy vector fonts. After the `sass` task is finished I replace links to small PNG files with Base64 encoding to inline them in my CSS files.

You should comment out tasks, we haven’t written until now, or Gulp can not run. I just included them so we don’t need to come back for each task we write and add a line.

## Delete Assets

To wipe out all files in the asset folder I use the node module `del`.

```bash
$ npm install --save-dev del@0.1.3
```

I need to add a config for deleting:

#### gulp/config.js

```javascript
browsersync: {
...
},
delete: {
  src: [developmentAssets]
}
```

I will shorten all configuration options from now on. Every task will have its own option section. These are JavaScript objects so don’t forget the trailing comma if you add a new configuration option.

The actuall task will look like this:

#### gulp/tasks/development/delete.js

```javascript
var gulp = require("gulp");
var del = require("del");
var config = require("../../config").delete;

/**
 * Delete folders and files
 */
gulp.task("delete", function (callback) {
  del(config.src, callback);
});
```

If you use a newer version of `del` or run into trouble, because `del` doesn’t finish, try deleting the `callback` from the function.

## Jekyll

Next, I will write the configuration and the task to create the Jekyll site:

#### gulp/config.js

```javascript
jekyll: {
  development: {
    src:    src,
    dest:   development,
    config: '_config.yml'
  }
}
```

#### gulp/config/development/jekyll.js

```javascript
var gulp = require("gulp");
var cp = require("child_process");
var browsersync = require("browser-sync");
var config = require("../../config").jekyll.development;

/**
 * Build the Jekyll Site
 */
gulp.task("jekyll", function (done) {
  browsersync.notify("Compiling Jekyll");

  return cp
    .spawn(
      "bundle",
      [
        "exec",
        "jekyll",
        "build",
        "-q",
        "--source=" + config.src,
        "--destination=" + config.dest,
        "--config=" + config.config,
      ],
      { stdio: "inherit" }
    )
    .on("close", done);
});

gulp.task("jekyll-rebuild", ["jekyll"], function () {
  browsersync.reload();
});
```

There is a gulp plugin for Jekyll, but it’s alpha and was blacklisted because it’s not needed as you can run shell tasks with node. But I have to send the `done` status when the task is finished.

All this task is doing is running `jekyll build` with some options. I use `app` as the source folder, `build/development` as the target, and point to my `_config.yml`.

I put my `_config.yml` and other configuration files always at the root of my project. If you don’t like that, you need to update the configuration to point to the location of your `_config.yml`.

{% banner "To bundle or not to bundle" %}

**Be carefull:** If you didn’t install Jekyll with a Gemfile you’ll have to change the Jekyll tasks and remove the `bundle exec` part. Instead of `return cp.spawn('bundle', ['exec', 'jekyll' …` you write `return cp.spawn('jekyll', ['build', '-q' …`. All other options stay the same.

{% endbanner %}

I have a second Jekyll build task `jekyll-rebuild`, which is only a wrapper for a rebuild. All it does is reloading the Browser when the build is completed.

## Conclusion

This concludes the 3rd part of my series _Introduction to Gulp.js_. We learned how to run files in specified order with `run-sequence`, how to delete files and folders, and how to execute a shell task like Jekyll.

{% more "View Source on GitHub", "https://github.com/kogakure/gulp-tutorial", true %}
