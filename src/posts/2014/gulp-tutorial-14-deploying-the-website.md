---
title: "Introduction to Gulp.js 14 – Deploying the Website with Rsync"
author: Stefan Imhoff
date: 2014-10-31T08:00:00+02:00
description: "The ultimative tutorial and guide for Gulp.js: How to deploy your website with rsync to your server."
tags:
  - code
series:
  - gulp
---

This is the 14th of my series _Introduction to Gulp.js_. Today I will write a task to sync the files of my Jekyll site to my webserver.

## Deploying the Website

There are a lot of possibilities to get a website on the server. You may use FTP, SFTP, SCP, SCP2, Rsync, or Git. I use Rsync because it’s fast and easy to use.

I write another task as an entry point: `deploy`

#### gulp/tasks/deploy.js

```javascript
var gulp = require("gulp");

/**
 * Start rsync task
 */
gulp.task("deploy", ["rsync"]);
```

This will just start the `rsync` task. But I could add more tasks, for example, a task, which creates a zip archive of the build and copies it to a backup on my hard drive.

```bash
$ npm install --save-dev gulp-rsync@0.0.5
```

#### gulp/config.js

```javascript
rsync: {
  src: production + '/**',
  options: {
    destination: '~/path/to/my/website/root/',
    root: production,
    hostname: 'mydomain.com',
    username: 'user',
    incremental: true,
    progress: true,
    relative: true,
    emptyDirectories: true,
    recursive: true,
    clean: true,
    exclude: ['.DS_Store'],
    include: []
  }
}
```

This task will grab all files in my production folder, connect to my server, and copy all files recursively to my website root. It will delete old files and just add changes to the server.

#### gulp/tasks/production/rsync.js

```javascript
var gulp = require("gulp");
var rsync = require("gulp-rsync");
var config = require("../../config").rsync;

/**
 * Copy files and folder to server
 * via rsync
 */
gulp.task("rsync", function () {
  return gulp.src(config.src).pipe(rsync(config.options));
});
```

## Conclusion

This concludes the series _Introduction to Gulp.js_. Developing and deploying with Gulp.js is fun.

I like the UNIX philosophy of Gulp.js: Having small files, which do one task and connect these to larger workflows. And because I kept my Gulp.js tasks small, pluggable, and easily shareable, I was able to add Gulp.js to my second website in less than five minutes.

{% more "View Source on GitHub", "https://github.com/kogakure/gulp-tutorial", true %}
