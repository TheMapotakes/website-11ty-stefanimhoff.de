---
title: "Introduction to Gulp.js 16 – PostCSS"
author: Stefan Imhoff
date: 2015-10-16T07:50:29+00:00
description: "The ultimative tutorial and guide for Gulp.js: How to use PostCSS with Gulp to process CSS and how to lint your CSS files with Stylelint."
tags:
  - code
series:
  - gulp
---

This is the 16th part of my series _Introduction to Gulp.js_. Today I will show how to use PostCSS to process CSS files. I will replace Ruby Sass with PostCSS and additionally show how to lint stylesheets automatically in the background while developing with Stylelint.

## Compass, Sass, LESS, Stylus … why do we need more tools?

I use Sass and SCSS now for a long time, started with _Compass_, moved to _Ruby Sass_ and wanted to move to _libSass_, but dependencies to some of my beloved Gems held me back.

There are a lot of Preprocessors, Libraries, Frameworks, which extend CSS. And a lot of them do good work and I didn’t want to miss Variables, Nesting or Mixins. But Ruby Sass and Compass in particular are **slooooooooow**, because Ruby is slow. Compiling my websites styles took 7-8 seconds, but I know of projects where one change will trigger a recompile, which takes **more than a minute**!

## What is PostCSS and why should I use it?

There is a new kid on the block: [PostCSS](https://github.com/postcss/postcss). I don’t care, if it’s Preprocessor, a Postprocessor or just a Processor. You write something, it will process your stuff and it will put out CSS.

Why should you use a new tool, if Sass and it’s competitors do their job? Because it’s **fast** ([3-30 times faster](https://github.com/postcss/benchmark)), **modular** and **extendible**. I bet you only need a small fraction of your Preprocessors functionality.

With PostCSS you can choose what you need from currently over [200 plugins](https://www.postcss.parts/). If you don’t find the plugin you need, you can write your own [plugin](https://github.com/postcss/postcss/blob/master/docs/guidelines/plugin.md) or [syntax](https://github.com/postcss/postcss/blob/master/docs/syntax.md).

**You get it all**: Variables, Mixins, Extends, Color Helpers, Fallbacks, Optimizations, Grids … you choose. You can even start using future CSS syntax today, let PostCSS transpile it for you.

I swapped out _Ruby Sass_ with PostCSS and my CSS is now transformed in 2-3 seconds. Go and take a look at my [beautiful new code](https://github.com/kogakure/jekyll-stefanimhoff.de). I use a Responsive Typography, Autoprefixer, and the awesome [LostGrid](https://github.com/peterramsing/lost).

## PostCSS

In this part of my series, I will rip out Ruby Sass and add PostCSS instead. So follow me along …

First I will install all needed Node modules:

```bash
$ npm install gulp-postcss@6.0.1 precss@1.2.3 gulp-cssnano@2.0.0 gulp-util@3.0.6 autoprefixer@6.0.3 css-mqpacker@4.0.0 --save-dev
```

I only add some basic plugins, but you can add more if you like later. I add [gulp-postcss](https://github.com/postcss/gulp-postcss), [precss](https://github.com/jonathantneal/precss) (which will allow Sass-like syntax), [gulp-cssnano](https://cssnano.co/) (which will compress and optimize the CSS), [autoprexifer](https://github.com/postcss/autoprefixer) for automatic vendor prefixes and [css-mqpacker](https://github.com/hail2u/node-css-mqpacker) for combining media queries.

Next I will add the configuration for the new task:

#### gulp/config.js

```javascript
styles: {
    src:  srcAssets + '/styles/*.css',
    dest: developmentAssets + '/css',
    options: {
      precss: {},
      autoprefixer: {
        browsers: [
          'last 2 versions',
          'safari 5',
          'ie 8',
          'ie 9',
          'opera 12.1',
          'ios 6',
          'android 4'
        ],
        cascade: true
      },
      mqpacker: {}
    }
  },
```

I add the new task to my `development` folder:

#### gulp/tasks/development/styles.js

```javascript
var gulp = require("gulp");
var postcss = require("gulp-postcss");
var precss = require("precss");
var nano = require("gulp-cssnano");
var plumber = require("gulp-plumber");
var sourcemaps = require("gulp-sourcemaps");
var gutil = require("gulp-util");
var browsersync = require("browser-sync");
var autoprefixer = require("autoprefixer");
var mqpacker = require("css-mqpacker");
var config = require("../../config");

function onError(err) {
  gutil.beep();
  console.log(err);
  this.emit("end");
}

/**
 * Run CSS through PostCSS and it's plugins
 * Build sourcemaps and minimize
 */
var processors = [
  precss(config.styles.options.precss),
  autoprefixer(config.styles.options.autoprefixer),
  mqpacker(config.styles.options.mqpacker),
];

gulp.task("styles", function () {
  browsersync.notify("Transforming CSS with PostCSS");

  return gulp
    .src(config.styles.src)
    .pipe(
      plumber({
        errorHandler: onError,
      })
    )
    .pipe(sourcemaps.init())
    .pipe(postcss(processors))
    .pipe(nano())
    .pipe(sourcemaps.write("."))
    .pipe(gulp.dest(config.styles.dest));
});
```

### Rename SCSS files and folders

I rename the `scss` folder to `styles` and create inside of this folder a new folder `partials`. I rename all `.scss` files to `.css` and move them into the partials folder (except the `main.css`). I recommend to comment out all lines in this file for now and bring them back later. Otherwise it will not run properly, because some of the syntax is different.

### Update the Code

Next, I will need to replace some lines of the old `sass` task with my new `styles` task. If you have never seen a **DIFF** file: a `-` (and red line) in front of a line has to be removed and a `+` (and green line) in front of a line has to be added:

#### gulp/config.js:132

```diff
- sass:    srcAssets + '/scss/**/*.{sass,scss}',
+ styles:  srcAssets + '/styles/**/*.css',
```

#### gulp/config.js:154

```diff
- css: srcAssets + '/scss/base/',
+ css: srcAssets + '/styles/partials/base/',
```

#### gulp/tasks/development/watch.js:9

```diff
- gulp.watch(config.sass,    ['sass', 'scsslint']);
+ gulp.watch(config.styles,  ['styles', 'scsslint']);
```

#### gulp/tasks/development/build.js:11

```diff
- 'sass',
+ 'styles',
```

#### gulp/tasks/production/build.js:10

```diff
- 'sass',
+ 'styles',
```

#### gulp/tasks/development/base64.js:8

```diff
- gulp.task('base64', ['sass'], function() {
+ gulp.task('base64', ['styles'], function() {
```

I remove these lines from my `optimize-css.js` task, because `cssnano` already does the job:

#### gulp/tasks/production/optimize-css.js:2

```diff
- var csso = require('gulp-csso');
```

#### gulp/tasks/production/optimize-css.js:7

```diff
- * Copy and minimize CSS files
+ * Copy CSS files
```

#### gulp/tasks/production/optimize-css.js:11

```diff
- .pipe(minifycss(config.options))
```

FontCustom has to be changed to copy the Vector fonts CSS to the new location and provide CSS instead of SCSS:

#### fontcustom.yml:34

```diff
- css: app/_assets/scss
+ css: app/_assets/styles
```

#### fontcustom.yml:48

```diff
- templates: [ scss, preview ]
+ templates: [ css, preview ]
```

After I updated the FontCustom config file I have to run the task for creating the Vector Fonts again:

```bash
$ gulp fontcustom
```

The syntax of the different PostCSS plugins is different. I use [PreCSS](https://github.com/jonathantneal/precss), which is a lot like Sass, but some things look a little bit different. To write down all of the changes I made to the CSS would extend the scope of this tutorial too much, as it’s a very long file. But you can have a look, how I refactored all CSS files with updated syntax in my [GitHub repository](https://github.com/kogakure/gulp-tutorial/commit/fc2398d933e2094832a00ac123b30c772269e08c). If you are interested how I replaced [Singularity](https://github.com/at-import/Singularity) (which is still the best Sass grid available) with [LostGrid](https://github.com/peterramsing/lost) and all the other things, look into [my websites source code](https://github.com/kogakure/jekyll-stefanimhoff.de).

You can run `gulp` again now, and PostCSS will process the styles.

## Linting CSS with PostCSS plugins

Of course we want to write clean and beautiful stylesheets. That’s why we should always run a Linter, which checks our CSS.

Until now a Gem did this job and was checking the SCSS syntax for errors. But now we use CSS and need a better (and faster) solution.

First I install the Node modules, which are needed:

```bash
$ npm install stylelint@1.2.1 postcss-reporter@1.3.0 --save-dev
```

Now I’ll add the configuration for Linting to my Gulp config:

#### gulp/config.js

```javascript
lintStyles: {
    src: [
      srcAssets + '/styles/**/*.css',
      '!' + srcAssets + '/styles/partials/_syntax-highlighting.css',
      '!' + srcAssets + '/styles/partials/_sprites.css',
      '!' + srcAssets + '/styles/partials/fontcustom.css'
    ],
    options: {
      stylelint: {
        'rules': {
          'string-quotes': [2, 'double'],
          'color-hex-case': [2, 'lower'],
          'value-no-vendor-prefix': 2,
          'declaration-no-important': 0,
          'rule-non-nested-empty-line-before': [2, 'always', {
            ignore: ['after-comment']
          }]
        }
      },
      reporter: {
        clearMessages: true
      }
    }
  },
```

I watch all CSS files, except the generated (FontCustom and Sprites) and the syntax highlighting file (which I don’t touch).

You can define linting rules for [stylelint](https://github.com/stylelint). These 5 rules are just an example, there are a [lot more](https://github.com/stylelint/stylelint/blob/master/docs/user-guide/rules.md). As CSS style is totally taste, you should pick the rules you like. `0` means ignore (default), `1` is warning and `2` is error. Some rules need additional options.

Next I will add the Gulp task:

#### gulp/tasks/development/lint-styles.js

```javascript
var gulp = require("gulp");
var postcss = require("gulp-postcss");
var stylelint = require("stylelint");
var reporter = require("postcss-reporter");
var config = require("../../config");

gulp.task("lint-styles", function () {
  return gulp
    .src(config.lintStyles.src)
    .pipe(
      postcss([
        stylelint(config.lintStyles.options.stylelint),
        reporter(config.lintStyles.options.reporter),
      ])
    );
});
```

All I need to do now is replacing the linting task in my watch task:

#### gulp/tasks/development/watch.js:9

```diff
- gulp.watch(config.styles,  ['styles', 'scsslint']);
+ gulp.watch(config.styles,  ['styles', 'lint-styles']);
```

If you run `gulp`, the task will lint your CSS files, whenever you save them and show errors with filename, line number and broken rule in the Terminal. PostCSS provides even a [plugin](https://github.com/postcss/postcss-browser-reporter) to bring the errors to your browser.

PostCSS will most likely have a bright future. Since it got popular quite a lot people got really exited. Companies like Google, Twitter, Alibaba, and Shopify already use PostCSS. And Bootstrap v5 will be most likely in PostCSS.

I’m sure we will see more really exiting Plugins in the future.

## Conclusion

This concludes the 16th part of my series _Introduction to Gulp.js_. We learned how to use PostCSS to process our CSS files and how to use Stylelint to lint the CSS files for errors.

{% more "View Source on GitHub", "https://github.com/kogakure/gulp-tutorial" %}
