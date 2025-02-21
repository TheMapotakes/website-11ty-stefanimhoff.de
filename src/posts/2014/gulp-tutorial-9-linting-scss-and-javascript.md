---
title: "Introduction to Gulp.js 9 – Syntax-Check of SCSS and JavaScript"
author: Stefan Imhoff
date: 2014-10-26T08:10:00+02:00
description: "The ultimative tutorial and guide for Gulp.js: How to check the syntax of SCSS and JavaScript files."
tags:
  - code
series:
  - gulp
---

This is the 9th part of my series _Introduction to Gulp.js_. Today I will use Gulp.js to automatically check my SCSS and JavaScript files for syntax errors and warnings.

I decided to lint my SCSS files and not the CSS files because it’s kind of pointless to lint generated CSS. But you can do this with [gulp-csslint](https://www.npmjs.com/package/gulp-csslint/).

```bash
$ npm install --save-dev gulp-scss-lint@0.3.6 gulp-jshint@1.8.5 jshint-stylish@2.0.1
```

Additionally, you’ll need to install the `scss-lint` Gem and run `bundle install`:

#### Gemfile

```ruby
source "https://rubygems.org"

gem 'jekyll', '~> 2.5.2'
gem 'sass', '>= 3.3'
gem 'scss-lint', '~> 0.31.0'
gem 'fontcustom', '~> 1.3.7'
```

Add some options for `jshint` and `scss-lint`:

#### gulp/config.js

```javascript
scsslint: {
  src: [
    srcAssets + '/scss/**/*.{sass,scss}',
    '!' + srcAssets + '/scss/base/_sprites.scss',
    '!' + srcAssets + '/scss/helpers/_meyer-reset.scss'
    ],
    options: {
      bundleExec: true
    }
},
jshint: {
  src: srcAssets + '/javascripts/*.js'
}
```

I ignore some files from checking (by adding a `!` in front of the path) because I didn’t write them or don’t have control over the syntax.

#### gulp/tasks/development/scss-lint.js

```javascript
var gulp = require("gulp");
var scsslint = require("gulp-scss-lint");
var config = require("../../config").scsslint;

/**
 * Lint SCSS files
 * `gem install scss-lint` needed
 */
gulp.task("scsslint", function () {
  return gulp.src(config.src).pipe(scsslint(config.options));
});
```

#### gulp/tasks/development/jshint.js

```javascript
var gulp = require("gulp");
var jshint = require("gulp-jshint");
var stylish = require("jshint-stylish");
var config = require("../../config").jshint;

/**
 * Check JavaScript sytax with JSHint
 */
gulp.task("jshint", function () {
  return gulp.src(config.src).pipe(jshint()).pipe(jshint.reporter(stylish));
});
```

{% banner "Configuration of Syntax Check Tools" %}

You may change the rules for linting SCSS or JavaScript by adding a hidden file `.scss-lint.yml` for SCSS lint and `.jshintrc` for JSHint to your project root.

To find out which options are available look into the documentation of [SCSS-Lint](https://github.com/brigade/scss-lint) and [JSHint](https://jshint.com/docs/).

{% endbanner %}

## Conclusion

This concludes the 9th part of my series _Introduction to Gulp.js_. Today we learned how to use Gulp.js to check the syntax of SCSS and JavaScript files. This task will run continuously while I write my files and print out errors to my console the moment I created them.

{% more "View Source on GitHub", "https://github.com/kogakure/gulp-tutorial", true %}
