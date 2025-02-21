---
title: "Introduction to Gulp.js 10 – Generating CSS Image Sprites"
author: Stefan Imhoff
date: 2014-10-27T07:40:00+02:00
description: "The ultimative tutorial and guide for Gulp.js: How to generate image sprite maps with Spritesmith."
tags:
  - code
series:
  - gulp
---

This is the 10th part of my series _Introduction to Gulp.js_. Today I will use Gulp.js to create CSS image sprites.

Just to be sure everybody knows what I’m talking about: A CSS image sprite is a collection of images put into a single image. This way fewer requests are needed and the website will load faster. The CSS file will move the image for each sprite to the correct position.

CSS images sprites are not used that often anymore, because of SVG or vector fonts. But I still use them as a fallback for browsers incapable of displaying vector fonts.

I will need a Spritesmith plugin for Gulp.js:

```bash
$ npm install --save-dev gulp.spritesmith@4.1.1
```

#### gulp/config.js

```javascript
sprites: {
  src: srcAssets + '/images/sprites/icon/*.png',
  dest: {
    css: srcAssets + '/scss/base/',
    image: srcAssets + '/images/sprites/'
  },
  options: {
    cssName: '_sprites.scss',
    cssFormat: 'css',
    cssOpts: {
      cssClass: function (item) {
        // If this is a hover sprite, name it as a hover one (e.g. 'home-hover' -> 'home:hover')
        if (item.name.indexOf('-hover') !== -1) {
          return '.icon-' + item.name.replace('-hover', ':hover');
          // Otherwise, use the name as the selector (e.g. 'home' -> 'home')
        } else {
          return '.icon-' + item.name;
        }
      }
    },
    imgName: 'icon-sprite.png',
    imgPath: '/assets/images/sprites/icon-sprite.png'
  }
}
```

I split my config into three subsections: The source files (individual icons for the sprite), the destination for the sprite, and the CSS partial and the options for the image sprite. I use a custom `cssClass` which will generate `:hover` states by naming the hover sprites with `-hover`.

#### gulp/tasks/development/sprites.js

```javascript
var gulp = require("gulp");
var spritesmith = require("gulp.spritesmith");
var config = require("../../config").sprites;

/**
 * Generate sprite and CSS file from PNGs
 */
gulp.task("sprites", function () {
  var spriteData = gulp.src(config.src).pipe(spritesmith(config.options));

  spriteData.img.pipe(gulp.dest(config.dest.image));

  spriteData.css.pipe(gulp.dest(config.dest.css));
});
```

In the end, I get two files: a partial `_sprites.scss` containing the class attributes and a sprite `icon-sprite.png` containing all images.

All development tasks are done now. We have got a running development server, tasks to create the Jekyll site, and all assets and tasks for linting, sprite, and vector font creation.

Next, I will write the tasks needed to get a production-ready code.

## Conclusion

This concludes the 10th part of my series _Introduction to Gulp.js_. Today we learned how to create CSS image sprites with Gulp.js and Spritesmith.

{% more "View Source on GitHub", "https://github.com/kogakure/gulp-tutorial", true %}
