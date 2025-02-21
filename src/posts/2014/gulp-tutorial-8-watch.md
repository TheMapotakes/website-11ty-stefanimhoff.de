---
title: "Introduction to Gulp.js 8 – Watch for Changes"
author: Stefan Imhoff
date: 2014-10-25T10:00:00+02:00
description: "The ultimative tutorial and guide for Gulp.js: How to set up a watch task, which triggers other tasks on file changes."
tags:
  - code
series:
  - gulp
---

This is the 8th part of my series _Introduction to Gulp.js_. Today I will set up watch tasks for many different files with Gulp.js.

Do you remember the `watch` task from the beginning? It just started BrowserSync and the development server until now but didn’t watch for anything. I will write these watch tasks now.

`Watch` is part of the API of gulp. It will watch files for changes, addition or deletion, and trigger tasks.

#### gulp/config.js

```javascript
watch: {
  jekyll: [
    '_config.yml',
    '_config.build.yml',
    src + '/_data/**/*.{json,yml,csv}',
    src + '/_includes/**/*.{html,xml}',
    src + '/_layouts/*.html',
    src + '/_plugins/*.rb',
    src + '/_posts/*.{markdown,md}',
    src + '/**/*.{html,markdown,md,yml,json,txt,xml}',
    src + '/*'
  ],
  sass:    srcAssets + '/scss/**/*.{sass,scss}',
  scripts: srcAssets + '/javascripts/**/*.js',
  images:  srcAssets + '/images/**/*',
  sprites: srcAssets + '/images/**/*.png',
  svg:     'vectors/*.svg'
}
```

I watch for a lot of different file types for Jekyll. Changes in configuration files, data files, layouts, includes plugin, posts, etc.

The Sass task will watch for changes in files with the suffix `sass` or `scss`. JavaScript gets triggered if I change some JavaScript files. You get the point.

#### gulp/tasks/development/watch.js

```javascript
var gulp = require("gulp");
var config = require("../../config").watch;

/**
 * Start browsersync task and then watch files for changes
 */
gulp.task("watch", ["browsersync"], function () {
  gulp.watch(config.jekyll, ["jekyll-rebuild"]);
  gulp.watch(config.sass, ["sass", "scsslint"]);
  gulp.watch(config.scripts, ["scripts", "jshint"]);
  gulp.watch(config.images, ["images"]);
  gulp.watch(config.svg, ["copy:fonts"]);
  gulp.watch(config.sprites, ["sprites"]);
});
```

I set up six watch tasks. Whenever a file of the Jekyll watch gets changed, deleted, or added, the `jekyll-rebuild` task gets executed. This task will run the Jekyll build and after it’s finished reload the page.

For `SCSS` files I run the `sass` tasks and additionally I run a `scsslint` task, which will check my files for syntax errors.

Changes in JavaScript files trigger the `scripts` tasks and a `jshint` task, which will check my files for syntax errors.

If I add, modify or delete an SVG file my vector fonts get recreated. And as a fallback for browsers without vector font support I create a PNG sprite map, whenever I change an image of the sprite. It would be possible to auto-create the PNG files of the SVG files with [gulp-svg2png](https://www.npmjs.com/package/gulp-svg2png/), but I have some additional design on the sprite images, that’s why I don’t use it.

I miss now three tasks: `scsslint`, `jshint`, and `sprites`.

## Conclusion

This concludes the 8th part of my series _Introduction to Gulp.js_. We learned how to use Gulp.js to watch for changes, deletion, or creation of files and how to trigger tasks. And the best part: This is part of the Gulp.js API. We don’t need any plugin.

{% more "View Source on GitHub", "https://github.com/kogakure/gulp-tutorial", true %}
