---
title: "New Website 2020 – Development"
date: 2020-07-28T08:00:00+02:00
author: Stefan Imhoff
description: This is the last of three parts in a series of essays about the process of creating my new website. This one shows the development process for my website.
tags:
  - code
---

## Development

I started coding with [React](https://reactjs.org/) in mid of 2018 and began to play with the thought of using React as a technology for the website. I had followed the development of the static website generator [Gatsby](https://www.gatsbyjs.org/) since version 1.0 in July 2017. In September 2018 Gatsby reached version 2.0 and was finally a good option. I selected Gatsby because of its huge community, fantastic plugin system, its utilization of React, GraphQL, and a static page generator.

This essay has no code examples, as it’s more a description of technological decisions I took and problems I ran into and how I solved them. The whole [source code](https://github.com/kogakure/website-gatsby-stefanimhoff.de) is publicly available on GitHub.

## Setup

Between the end of November and mid of December 2019, I worked on the basic setup of the project which took 28 hours. I created a fresh, empty [Gatsby](https://www.gatsbyjs.org/) project. I don’t remember which starter I used, but it was one of the most basic ones. The concept of [Gatsby Recipes](https://www.gatsbyjs.org/docs/recipes/) wasn’t released yet, I had to do all configuration manually.

Every project I work on these days needs [Prettier](https://prettier.io/) installed. I don't even want to remember the times before Prettier. Formatting and indenting is not an issue anymore you think about, you write your code and it gets automatically reformatted and re-arranged.

Next, I installed [Husky](https://github.com/typicode/husky), a tool to add Git hooks to a project that allows running specific commands before or after specific Git commands. In combination with [Lint-Staged](https://github.com/okonet/lint-staged) which allows running linters on Git staged files, it’s the best toolkit to prevent oneself from committing invalid or broken code.

I installed [Commitizen](https://github.com/commitizen/cz-cli) a tool that helps formatting Git commit messages with the [AngularJS commit message convention](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines). This creates good commits that can be identified later and encourages atomic commits—meaning not condensing a documentation change and a bugfix into one commit but separating them into two different commits. This allows later to identify what was changed and why. Nobody likes looking at a 2,000 line commit including changes all over a code-base.

I used [EditorConfig](https://editorconfig.org/), a nice convention for ensuring consistent coding styles on a project. It wouldn’t be necessary on my website, because I’m the only developer working on my website, but I like to have an industry-standard start even on private projects.

## TypeScript

Next I configured [TypeScript](https://www.typescriptlang.org/) in Gatsby. The setup is quick and easy, using the [gatsby-plugin-typescript](https://www.gatsbyjs.org/packages/gatsby-plugin-typescript/) plugin. I decided early I wanted to use TypeScript on my whole project, even though this would slow me down. I started learning TypeScript a few months earlier, but I wanted to invest in learning the technology, because the whole industry moves to TypeScript, including my current employer.

The initial start was a little rough because I had no idea how to properly structure a Gatsby/TypeScript project. But by looking into other [TypeScript starters](https://www.gatsbyjs.org/starters/?c=Language%3ATypeScript&v=2) I found good inspiration on how to do it properly.

The first two weeks were tough and I cursed a lot at TypeScript, but then the development got quicker and I could reuse a lot of what I learned.

## Linting

I installed tools for static testing, linters for JavaScript/TypeScript ([ESLint](https://eslint.org/)) and CSS ([Stylelint](https://stylelint.io/)). All linters check automatically every line of code I wrote before I commit them.

## Unit Testing

A good code-base should have proper unit test coverage which is why I installed and configured [Jest](https://jestjs.io/) as a testing framework and used [React Testing Library](https://testing-library.com/) for its fantastic set of testing utilities. Additionally, I added [Jest Axe](https://github.com/nickcolley/jest-axe) a Jest matcher to identify common accessibility problems.

Each of my components has a snapshot test that covers the Markup and CSS and an Axe accessibility test to catch preventable problems. A lot of people think Snapshot tests are a bad habit, but I disagree and during my development, I was proved correct. The snapshot makes sure you don’t accidentally break Markup or CSS without at least acknowledging it. This happened multiple times, so I will stick to this habit and let critics talk to the hand. 🤚 Interactive components have additional integration tests checking for the correct handling of interactions.

## Styled Components

I decided to use [Styled Components](https://styled-components.com/) as a CSS in the JS library. I long opposed to the idea of CSS in JS, and I still _am_ for libraries that don’t use CSS syntax. I **hate** writing CSS in objects or JavaScript syntax (`borderBottom: "10px"` instead of `border-bottom: 10px`). But using Styled Components makes the development of components a lot faster. And it’s convenient to generate TypeScript types automatically out of variants with `keyof typeof variant`.

I picked [Styled-System](https://styled-system.com/) as an additional library but ripped it out later in the process because it was over-engineering for my one-person website. I don’t need super-flexible components that can be changed on the fly by a developer. I’m the developer on the project, I know what I need.

## Theming

I used the [Theme Provider](https://styled-components.com/docs/api#themeprovider) of Styled Components to prepare my website for a light and dark theme (plus theme color variations). Coding the theme toggle was the hardest part of the whole development. It took me good two weeks to get a working theme switcher with a theme context running. Yet I didn’t know yet worse things would come, but more to that topic later.

## MDX

One of the reasons I was happy to use Gatsby on my new website was the support of [MDX](https://mdxjs.com/) (Markdown with JSX). Since the early days of blogging, I was always annoyed how hard it was to get custom things into a blog post. WordPress introduced the concept of [shortcodes](https://wordpress.com/support/shortcodes/) which was a big thing. But this is nothing compared to what MDX can do: MDX allows it to develop complex components doing what you like and then adding them as normal HTML tags in the Markdown documents. This allows for interactive posts or custom styled tags.

And the best thing is: With the help of the MDX Provider it’s possible to automatically provide every Markdown document with your custom components without the need of manually importing them every time. You can remap standard Markdown tags to your components.

## Blog

Next, I tried to get a basic blog running by adding sample MDX files and using [GraphQL](https://graphql.org/) to query the data and display it on the homepage. Nothing fancy, just to kick off things. I didn’t work any further on the blog for the next few months.

## Code Highlighting

As I write sometimes essays with code examples, I needed code highlighting. A common way of doing this is using a JavaScript library like [PrismJS](https://prismjs.com/). I added the library and the [Gatsby plugin](https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-remark-prismjs) but wasn’t happy with the solution. The library is big and slows down the loading. And all that to get code highlighted.

But then I found the community plugin [gatsby-remark-vscode](https://github.com/andrewbranch/gatsby-remark-vscode) and it’s magical. It allows you to use any [Visual Studio Code](https://code.visualstudio.com/) plugin to be used on your website: Themes or syntax highlighting are the most commonly used. You don’t need to rely on the library of JavaScript library to support your wished syntax. If Visual Studio Code supports it, it will be running on your website. I picked my favorite color theme and added it to my project. Additionally, the plugin has a huge amount of options, for example, changing themes matching the color scheme preferences or contrast preferences or highlighting specific lines or ranges. And the best thing is: Everything is pre-rendered, you don’t need to load one line of JavaScript for the code highlighting.

## Persistent Theme Toggle and SSR Rendering

The last thing I coded on Silvester's day 2019 was a persistent toggle, preselecting your color options by reading your systems preferences and saving your preferences to a persistent state consistent across multiple tabs.

But in mid-January, I recognized a strange behavior: My color theme didn’t work as expected after I build the Gatsby site for production. There was always a flash and styles didn’t change consistently across the whole website. I was nerve-wrecked after coding 3 weeks on the color scheme everything seemed now for nothing.

After debugging for a few days I could narrow down the problem to the SSR (Server-Side-Rendering) of Gatsby. Gatsby creates a static website of all your pages that will run without any JavaScript. JavaScript adds a better experience and a better response to a website. And to render everything to a static file, the default theme get’s picked (which was the light theme in my case). Even after React is loaded it won’t replace the light theme with the dark theme automatically. You would need to trigger a manual re-render of the whole page which looked as an unfavorable solution to me. Luckily other people ran into the problem and I found out that the easiest solution is to move the colors from the theme provider into [CSS Custom Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/--*) (aka CSS Variables).

In the end, I solved the problem by using a few global JavaScript functions to change the theme by applying a CSS class to the `body` tag and then switching the colors with the help of CSS variables. This works without any flaws and has a good performance. Plus: I didn’t need the Theme Provider any more and could remove it.

## SEO

In January of 2020, I created an SEO component. As long as I work with websites I know of the importance of an SEO-friendly website. If the website can’t be crawled properly it will be bad for disabled people, content crawlers (like [Pocket](https://getpocket.com/), or Feed Reeders) and sharing content on social media won’t work properly. Bad SEO: Your website sucks.

The component I created (I was inspired by a lot of other Gatsby developers) provides every page with the full set of necessary headers: Meta tags, title, description, OpenGraph and Twitter tags (for sharing on social media sites) and a lot of other things. The component allows changing any of these parameters on a per-page basis. A specific page shouldn’t be crawled? Add a different `robots` rule to that page.

## Easter Eggs

I started with coding the most unnecessary components first: The easter eggs. 😬 But as I wanted the website to have uncommon and never-seen-before features, this was important to me. Next to my favorite easter egg, I coded a `LocalizedDate` component, which allows the visitor to pick between three different date formats: US, German, or Japanese. Your selection will be remembered in your browser. I admit I did it because I like how the Japanese date looks like but didn’t want to torture my visitors with the format.

I coded in January the `ColorSwatch` component, a component that can display color and its values. I knew I would need this later.

## SVG Generation

I created a script to generate components from SVG illustrations using [SVGR](https://react-svgr.com/). It took me a little bit to find out how to make the script export TypeScript components, but could, fortunately, figure it out.

I use the beautiful and minimalistic [Remix Icons](https://remixicon.com/) and custom icons created by me, but try to use as few icons as possible.

## Web Font

The last thing I did in January 2020 was adding the web font to my website. I knew from the beginning I didn’t want any dependency on a font provider, and _certainly_ not Google. I removed all page tracking in 2019 from my website and don’t indent to add any tracking back. Adding a web font from Google Fonts will automatically bring user tracking. Additionally, the performance is never as good as if the font is directly served from your server.

That’s why I found a high-quality version of _Playfair Display_ (Google does something to the fonts) and converted them with [Font Squirrels](https://www.fontsquirrel.com/) free generator into all the necessary formats.

I use the [Web Font Loader](https://github.com/typekit/webfontloader) to asynchronously load the fonts. The library adds specific classes to the `html` tag, reflecting the different states of the web font loading. This allows swapping fonts or changing CSS depending on the state of the loading. I load initially the font _Georgia_ in a different font-weight that allows the minimal amount of jumping text when the web font is finished loading.

## Components, Components, and More Components

From February 2020 to April 2020 I did one repeating thing: Developing components. I added components for typography, links, code blocks, text styles, quotations, lists, footnotes, and many more. It was a tiring process, but I knew I would need all these components.

After a few components, I got tired of always copy and paste the folder structure of a component that I installed [Plop](https://plopjs.com/). Plop can be used to auto-generate any file and folder structure, including automatically naming everything.

## Component-Driven Development

I never worked with [Storybook](https://storybook.js.org/) before, but I always wanted to try it. Storybook is an open-source tool for developing UI components in isolation. It has a huge community creating plugins for every thinkable use case.

I moved all my existing components to [Storybook](https://styleguide.stefanimhoff.de/) and started with a component-driven development. I created one story (a use case) for every component variation. Then I developed the components in isolation of any website code in Storybook and unit tested the stories. This made the development much easier and fun than creating a manual [style guide](/styleguide/) page and developing the components on that.

## Grid

In mid of May 2020, I started developing the page grid, row, and column helpers. The underlying technology is _obviously_ CSS Grid Layout. I wanted the grid to be as flexible as possible: Supporting explicitly placed grid items, but different size presets for simple pages and allowing content to be auto-placed on the grid if possible.

I ran into a problem because all the designs I created for the huge Desktop size of 1800 Pixels. Each grid module was 100 × 100 Pixels wide. But a responsive page doesn’t care about pixel sizes.

## Responsive Grid and Typography

Lucky I stumbled upon a video about [min(), max(), and clamp()](https://youtu.be/U9VF-4euyRo) a few weeks earlier. I’ve had never heard of `clamp` before and was surprised how supported it was. IE11 doesn’t support it, but _who_ cares about _that_ crappy discontinued browser? Not even Microsoft.

The combination of `min()`, `max()` and `clamp()` allows responsive typography or grids, that respect a minimal and maximal size and adjust the size of anything in between these two values matching to the available screen space. This changed everything! I switched all font-sizes and space tokens to clamp. This allows the grid, margins, padding, or font-sizes to adjust to the screen size but stay inside reasonable ranges.

## Pages

At the end of May 2020, I started creating the first page: [The Error 404 page](/404/). I used explicitly positioned content and could test out my grid.

Next, I created a first Markdown page: [Imprint](/imprint/). And then the [About](/about/) page, another explicitly designed page. It was impressive how quick and easy it was to create all the layouts I designed months earlier in Affinity Designer. Each page took me a day. I created the first parts of the [Homepage](/), another explicitly set site.

Then I started with my first data-driven pages: [The Traditional Colors of Japan](/traditional-colors-of-japan/) and [Haiku](/haiku/).

I created the colors a few years ago from a Japanese book about colors. Back then it was a blog post, but I always intended to create a full-page around that presenting all the colors on a space the deserved. I read all the color values from a YAML file and generate the page with the help of GraphQL with Gatsby using my Color Swatch component I developed a few months earlier.

My Haiku (Japanese poetry) were always cramped on one boring page on my previous website and I wanted to present them properly. I created a proper introduction page with an explanation and information and listing all my Haiku. I generated for each Haiku an individual page, including an English translation of my in German written Haiku.

In Mid of June I started creating the [Journal](/journal/) aka Blog. I created a two-column layout with all essays sorted alphabetically on the left side and all monthly recommendations sorted by date on the right side. As the date is for my essays of minor importance (except the recommendations), alphabetical sorting is the best option. I created the Journal detail page, displaying an individual essay.

I created a sophisticated `Row` component a month earlier that can automatically align content by a fixed set of rules and needed to make it work on small screen sizes.

I thought long about dropping the [Projects](/projects/) page from my list for the initial launch of the new design because I had already worked on the project for a long time. But then I decided to do it and invest in the two additional weeks because now it is always the right time to do things. I not only created a Projects page but Showcases for selected projects.

While showing all my projects, I decided to move my [Sketchnotes](/sketchnotes/) to a dedicated section on the website.

## Migrating the Content

I knew one last big task was left on my list, that was tedious work but needed to be completed: Moving all essays to my new website. This took me a whole week because I decided to correct the spelling of every essay, needed to replace custom HTML with my components, and update the YAML Frontmatter metadata of each blog post. I decided to improve the quality of the images when possible.

## Recent Essays on the Homepage

The homepage was missing a design for the recent essays. My early designs of that section didn’t please me anymore, they were too aligned, boring, and blunt. I decided to experiment with a Grid automatically reordering itself and content randomly offset. The idea worked but the randomness will only reposition the recent blog essays each time I deploy my website as the SSR will embed the styles and prevent a new random positioning on each reload.

## Animations & Transitions

The last bigger tasks on my list where related to Animations and Transitions. I had moved these to the end of the development, to be able to ship without them, if it would get too complicated or time-intensive.

I used [React Transition Group](https://github.com/reactjs/react-transition-group) and [gatsby-plugin-layout](https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-plugin-layout) to be able to transition between different pages. I kept the transition simple and pleasant to stick to the principles of [Shibui](/new-website-2020-inspiration/).

I used [gatsby-plugin-scroll-reveal](https://github.com/solublestudio/gatsby-plugin-scroll-reveal) to reveal content when scrolling into the viewport. The plugin uses [Sal.js](https://mciastek.github.io/sal/) as a scroll animation library.

In the end, I improved the performance of all transitions on my website by changing what I transitioned. Early on I had quickly added simple transitions but got a low-performance warning from Jest.

## Progressive Web App & RSS/Atom Feed

The last thing I added to my website before I got ready to launch it was converting my website into a PWA (Progressive Web App) by adding a Service Worker. With Gatsby, this can be achieved with the plugins [gatsby-plugin-manifest](https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-plugin-manifest) and [gatsby-plugin-offline](https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-plugin-offline).

The last task on my list was adding an RSS/Atom Feed. I thought this would be an easy task, but I was wrong. I tried using the plugin [gatsby-plugin-feed](https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-plugin-feed), but couldn’t get it running. I tried another community plugin forked to support MDX, but ran into the same problems. The content didn’t want to render properly, all my custom MDX components were missing. Debugging was hard because the feed gets _only_ generated with a production build, which is why it took me multiple hours to debug the issue. In the end, I found out by accident I had to move the MDX Provider from my layout component to the `wrapRootElement` function. Lastly, I changed a few of my components to be better displayed in the RSS feed.

## Continuous integration & Deployment

I use [Netlify](https://www.netlify.com/) to host my website. When I open a pull request on [GitHub](https://github.com/kogakure/website-gatsby-stefanimhoff.de) Netlify creates automatically a preview deploy on a custom URL. This is a fantastic feature and the main reason I selected Netlify. An automation workflow checks the Redirect file, Mixed content, and other changes.

I use [Travis CI](https://travis-ci.org/github/kogakure/website-gatsby-stefanimhoff.de) to run the CSS and JavaScript linting and the automated tests with Jest (including coverage). The coverage report gets automatically uploaded to [Codecov](https://codecov.io/gh/kogakure/website-gatsby-stefanimhoff.de/).

I added the [Renovate bot](https://renovate.whitesourcesoftware.com/) to automatically open pull requests for package upgrades but removed it later because it would quickly use up my free build minutes on Netlify.

## Source Code

If you’re interested in how I developed each step of the website, I created nearly 500 Git commits. You can check out the [source code](https://github.com/kogakure/website-gatsby-stefanimhoff.de) of the website at any selected time and look into how the website looked.

---

This is the last of _three_ parts in a series of essays about the process of creating my new website.

1. [Inspiration](/new-website-2020-inspiration/)
2. [Design](/new-website-2020-design/)
3. **Development**
