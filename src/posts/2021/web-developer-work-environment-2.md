---
title: Setting Up a Web Developer Work Environment – Neovim
date: 2021-12-23T10:00:00+01:00
author: Stefan Imhoff
description: I decided to re-exampine my developer tools and replace some of them with better tools.
tags:
  - code
  - personal
  - productivity
  - software
---

As I mentioned in my introduction, I use Vim for a long time. My shortcuts and commands have carved their way into my memory and I don’t even need to think about 80 % of my shortcuts. Sometimes I can’t even recall them without doing them.

The **Stack Overflow Developer Survey 2021** lists [Neovim](https://neovim.io/) as the <q>most loved editor [and] the 10th most wanted editor.</q>[^stackoverflow2021] And this has a reason. Vim is fantastic on its own, but Neovim adds features we know from code editors like Visual Studio Code. And it is fast, super-fast. What editor (besides Emacs) can load a 50 GB large log file without crashing?

Learning Vim looks hard, people even joke the hardest part is to exit Vim. But don’t forget, nobody knows all features of Vim. But the constant learning of new cool things keeps you motivated for years and you’ll fall in love with the editor. If you _only_ know regular editors, you have no idea what you’re missing. Fast, precise movement and editing without ever lifting the fingers of the keyboard.

## Learning Vim

I won’t go into a long list of tutorials and screencasts, but it’s easy to find these resources [with one search](https://search.brave.com/search?q=learning+vim). There are uncounted articles, books, screencasts, or games available to learn Vim.

These tutorials and videos are a good start:

- [Learn Vim For the Last Time: A Tutorial and Primer](https://danielmiessler.com/study/vim/)
- [5 Minutes Vim](https://www.barbarianmeetscoding.com/series/5-minutes-vim/)
- [Exploring Vim](https://www.barbarianmeetscoding.com/series/exploring-vim/)
- [Mastering the Vim Language](https://youtu.be/wlR5gYd6um0)
- [Learning Vim in a Week](https://youtu.be/_NUO4JEtkDw)
- [How to Do 90% of What Plugins Do (With Just Vim)](https://youtu.be/XA2WjJbmmoM)

The number of plugins can be overwhelming, there are always three alternatives (because developers like options).

## Good Defaults

The good thing is Neovim and its plugins come with reasonable defaults. You don’t need to configure a lot to get started. If you develop wishes later, you can always use a custom configuration.

These are things I learned by accident. When I reviewed my old `.vimrc` I decided to check each setting in the documentation. Searching for `:h mouse` will show you the configuration options for mouse usage. If the setting has a default, I removed it from my configuration file (`mouse` is not). This reduced my [settings](https://github.com/kogakure/dotfiles/blob/master/nvim/settings.vim) to around 75 lines.

I decided to move full-in Neovim and be backward-incompatible with Vim and started using [init.vim](https://github.com/kogakure/dotfiles/blob/master/nvim/init.vim) instead of `.vimrc`. Once the Vim APIs in Lua is completed and stable, I might migrate to Lua.

## Mappings

Mappings are one of the reasons people love Vim. Instead of having a developer decide what keyboard shortcut you have to use (or giving you the option to remap everything), Vim allows you to create shortcuts for everything. You can use typical keys as <kbd>⇧</kbd>, <kbd>⌃</kbd>, <kbd>⌥</kbd>, or <kbd>⌘</kbd> as in any other software. But you’ll soon run out of options.

Vim introduced the concept of a **Leader** key. A key you can press (default <kbd>\\</kbd>) and after that, it will wait for a combination of more keys to execute a command. This can be one letter or even a full word. I use for example this mapping to turn spell checking on or off:

```vim
nnoremap <silent> <leader>rs :set spell!
```

My leader key is remapped to the <kbd>space</kbd> bar. If I press <kbd>space</kbd> followed by <kbd>r</kbd><kbd>s</kbd> it will execute the command. The `!` switches the previous value to its opposite.

I use mnemonic aids for my key combinations. This might be `pf` (pretty format) or `cf` (current file) for example.

If you want to see all my regular mappings, please look at the [mappings.vim](https://github.com/kogakure/dotfiles/blob/master/nvim/mappings.vim) file. I keep the mappings for plugins now in separate files for each plugin.

## Functions

You can write functions in Vim Script or Lua and bind them to shortcuts. I cleaned up and removed all functions I never used, so my [functions.vim](https://github.com/kogakure/dotfiles/blob/master/nvim/functions.vim) file is short.

## Auto Commands

Auto commands are commands that get executed automatically if a specific condition matches. This can be entering a file or mode, loading a specific file type, saving a file, or much more.

Previously, I had more than 150 lines of auto commands, but I decided to remove a lot of them. A lot of my auto commands were setting the right syntax highlighting for specific file types. I _guess_ with [Treesitter](https://neovim.io/doc/treesitter/) this should not be necessary anymore. My [autocmd.vim](https://github.com/kogakure/dotfiles/blob/master/nvim/autocmd.vim) file has a few settings for my used languages.

## Plugins

Neovim has many features inherited from Vim and Vi, but the developers can’t add all features. This is where plugins help out. The amount of Vim plugins is uncountable. The tag `vim-plugin` or `vim-plugins` matches nearly 1800 repositories on GitHub (but a lot of the best plugins aren’t even tagged). Whatever your wish is, somebody has written a plugin.

### Plugin Managers

Even for installing plugins, you have multiple options: You can download and copy the file manually (nobody does this) or install one of the many plugin managers. I use [vim-plug](https://github.com/kogakure/dotfiles/blob/master/nvim/autocmd.vim). All you need to do is add a line pointing to the plugin to your configuration, reload Vim, and run `:PlugInstall`.

![Vim Plug](/assets/images/posts/work-environment-plug-install.jpg)

### Plugin Configuration

I decided on my new repository to keep all configuration, settings, mappings, functions, and auto commands for a plugin in a dedicated file and load this file from one central file. This way no unnecessary lines stay around should I replace a plugin with a better plugin in the future.

Additionally, I can write the configurations in Vimscript or Lua, depending on the plugin, and don’t need to use the ugly syntax to embed Lua in Vimscript:

```lua
lua << EOF
-- Some Lua code
EOF
```

The amount of my plugins is currently 78. I will use the rough categories of my [plugins.vim](https://github.com/kogakure/dotfiles/blob/master/nvim/plugins.vim) file to group the plugins in this article. I’ll not mention all plugins. If you’re interested in what current plugins I use, please refer to the file above.

### Language Server Protocol

The built-in LSP client of Neovim is a fantastic feature. But it’s the most complicated part to set up at the moment. Of all the plugins, I struggled the most in setting up LSP. The main plugins to help are [lspconfig](https://github.com/neovim/nvim-lspconfig) and [nvim-lsp-installer](https://github.com/williamboman/nvim-lsp-installer) that will help to create configurations for the LSP servers and to install them without additional manual work.

![LSP Installer](/assets/images/posts/work-environment-lsp-installer.jpg)

One of the LSP servers is the diagnostics language server. There is an [unofficial collection](https://github.com/creativenull/diagnosticls-configs-nvim) of configurations for useful things as prettier or eslint.

[Trouble](https://github.com/folke/trouble.nvim) is a cool plugin to show errors, warnings, or hints for your code.

### Completion

Vim has a nice completion engine with `omnicomplete`, but with [nvim-cmp](https://github.com/hrsh7th/nvim-cmp) the feeling feels much more modern. The recommended setup includes more plugins by the same author to complete LSP, buffers, path, vim’s command line, and snippets of one of the four biggest snippet plugins.

![Completion](/assets/images/posts/work-environment-completion.jpg)

I decided to stay with [Ultisnips](https://github.com/SirVer/ultisnips) because it’s the most starred, creating snippets is quick and easy and with [vim-snippets](https://github.com/honza/vim-snippets) it brings a huge collection of snippets. There is a [source for nvim-cmp](https://github.com/quangnguyen30192/cmp-nvim-ultisnips).

The plugin [lspkind-nvim](https://github.com/onsails/lspkind-nvim) will add pretty pictograms for LSP completions as known from Visual Studio Code.

### File Management & File Editing

[Telescope](https://github.com/nvim-telescope/telescope.nvim) is my favorite plugin. It is a plugin to find, filter, preview, and pick files. It supports builtin dozens of features, for example, files, Vim features, LSP, Git, and many more. But it can be extended to browse [the file system](https://github.com/nvim-telescope/telescope-file-browser.nvim), [fzf](https://github.com/nvim-telescope/telescope-fzf-native.nvim), [recently used files](https://github.com/nvim-telescope/telescope-frecency.nvim), [node_modules](https://github.com/nvim-telescope/telescope-node-modules.nvim), or [browser bookmarks](https://github.com/dhruvmanila/telescope-bookmarks.nvim).

![Telescope](/assets/images/posts/work-environment-telescope.jpg)

I don’t use Tree plugins much, because Telescope is fast and finds everything, but sometimes you want to browse a specific folder structure. NerdTree is the most famous tree plugin, but I use a newer version written in Lua: [nvim-tree.lua](https://github.com/kyazdani42/nvim-tree.lua).

In Visual Studio Code I liked the GitLens plugin that showed on each line the author of the last commit. The plugin [blamer.nvim](https://github.com/APZelos/blamer.nvim) will add a similar functionality.

Multiple cursors are a fantastic feature that is not a default option in Neovim. But the [vim-visual-multi](https://github.com/mg979/vim-visual-multi) plugin can add this feature.

The plugin [nvim-autopairs](https://github.com/windwp/nvim-autopairs) will add support for automatic pairing characters, including options to configure the behavior of the cursor position.

Vim has a mark feature built-in, but I use an additional bookmark plugin for a long time now: [vim-bookmarks](https://github.com/MattesGroeger/vim-bookmarks). It shows the marks with pretty pictograms, additionally a comment can be added to the mark and they can be saved automatically on the hard drive and restored later.

One of the most interesting Twitch streamers in the Neovim community created the [Harpoon](https://github.com/ThePrimeagen/harpoon) plugin. It is a tool to quickly jump between a handful of files you’re currently working on. When I saw it first, I thought why should I need this, but now I don’t want to miss it.

[File-line](https://github.com/bogado/file-line) is a small plugin I use for many years. It allows to directly open a file and jump to a specific line by adding the line number to the filename, e. g. `vim index.html:20`.

I love [Tabular](https://github.com/godlygeek/tabular) and use it for many years. It allows aligning content on specific characters. I found a function that even realigns automatically when a <kbd>|</kbd> is typed which is helpful for Markdown tables.

Another old plugin I use all the time is [VisIncr](https://github.com/vim-scripts/VisIncr). It allows to visually increase numbers of all kinds on multiple lines at once.

[Gitsigns](https://github.com/lewis6991/gitsigns.nvim) is a plugin that integrates Git decorations to Neovim. Additions, Deletions, Changes are highlighted with a color bar at the side. It is possible to stage code from inside Neovim.

Finding and replacing multiple files is not that intuitive in Vim. Luckily I found [Far](https://github.com/brooth/far.vim), a pretty plugin that simplifies the interface.

I work with GitHub Enterprise at work and need to share code positions with other developers all the time. The plugin [vim-gh-line](https://github.com/ruanyl/vim-gh-line) is made for this. In Vim I position my cursor on a code line and press a shortcut to open the same position in GitHub.

It should be possible to use LSP to run Prettier on Neovim, but I couldn’t yet figure out how. That’s why I installed [neoformat](https://github.com/sbdchd/neoformat) for now to do the job for me.

[Copilot.vim](https://github.com/github/copilot.vim) get’s an honorable mention, because I can’t use it yet. [GitHub Pilot](https://copilot.github.com/) is a new feature by GitHub and it's currently invite-only and needs an API key at the moment. I’m on the waiting list. It is a new AI feature that learns automatically and will suggest the right code at the right moment. Looks promising!

### Window Improvements

A lot of new Vim users get confused: Vim supports buffers, windows, and tabs. A buffer is the instance of a file, a window is a view on a buffer and a tab is a collection. Because buffers are not visible by default, beginners don’t know how to see all open files. [Bufferline](https://github.com/akinsho/bufferline.nvim) will add a feature to show all open buffers as if they would be tabs.

Another thing on the opposite end of the window that many people want is a status line that shows all kinds of information: Vim mode, Git branch name, errors, file type, language, time, column, or many more. [Lualine](https://github.com/nvim-lualine/lualine.nvim) adds a pretty bar that brings these features and can be configured to meet your needs.

[Cursorline](https://github.com/yamatsum/nvim-cursorline) is a plugin that will highlight the current word and all occurrences automatically without searching for the word.

[Neoscroll](https://github.com/karb94/neoscroll.nvim) is a plugin that adds smooth scrolling to Neovim.

I like to work distraction-free. In Visual Studio Code there is the Zen mode which will hide all unnecessary things. For Vim the plugin [Goyo](https://github.com/junegunn/goyo.vim) does a similar thing. It centers the editor and removes all distractions.

![Goyo](/assets/images/posts/work-environment-goyo.jpg)

### Syntax Highlighting

Treesitter is the newly builtin parsing library of Neovim. Syntax highlighting was always a hassle because syntax plugins were built with Regular Expressions. Treesitter understands the code semantically and will be able to support many cool new features in the future. The list of supported languages is impressive. The [Treesitter](https://github.com/nvim-treesitter/nvim-treesitter) plugin allows easy install of languages.

But in case a language is not yet supported I use [Vim Polyglot](https://github.com/sheerun/vim-polyglot). The plugin supports 598 languages and only loads the needed ones for a file.

The only problem I have is support for Styled Components. It’s an [open issue on Treesitter](https://github.com/nvim-treesitter/nvim-treesitter/issues/1111) and the [official plugin](https://github.com/styled-components/vim-styled-components) is unmaintained and doesn’t support new syntax as transitional properties which creates ugly linting problems for me. I hope this problem can be resolved soon.

As a Front-end Web Developer, I love to see my color values visualized. The plugin [colorizer.lua](https://github.com/norcalli/nvim-colorizer.lua) does this fast and pretty.

![Colorizer](/assets/images/posts/work-environment-colorizer.jpg)

Another plugin I use is [Emmet](https://github.com/mattn/emmet-vim). It allows using this simple syntax to automatically create HTML or CSS.

For Markdown I use the [Pandoc](https://github.com/vim-pandoc/vim-pandoc) and [Pandoc Syntax](https://github.com/vim-pandoc/vim-pandoc-syntax) plugins. They make writing Markdown enjoyable.

### Custom Text Objects

The most powerful feature of Vim was part of Vi. Vim has its own “language” to move around and edit text objects. It is for example possible to change a complete paragraph with <kbd>c</kbd><kbd>i</kbd><kbd>p</kbd> (change inner paragraph). The base language has support for a lot of text objects, but plugins can extend the language even further.

I use text object for [indent](https://github.com/michaeljsmith/vim-indent-object), [xml attributes](https://github.com/whatyouhide/vim-textobj-xmlattr), [date and time](https://github.com/kana/vim-textobj-datetime), [URIs](https://github.com/jceb/vim-textobj-uri), or [comments](https://github.com/glts/vim-textobj-comment).

### Custom Motions

I love sorting things. Vim has a sort feature built-in, that can even remove duplicates (`:sort u`). But you need the visual mode to mark the content you want to sort. The plugin [vim-sort-motion](https://github.com/christoomey/vim-sort-motion) allows sorting with a few keystrokes with text objects.

The plugin [exchange.vim](https://github.com/tommcdo/vim-exchange) allows switching two words without the visual mode.

Vim allows to jump to nearly every position with a few keystrokes, but [vim-easymotion](https://github.com/easymotion/vim-easymotion) increases the precision to directly jump to any letter.

### Tim Pope Plugins

Tim Pope is one of the most famous plugin creators. He created nearly 50 plugins, a few the most used by the community.

I use [vim-surround](https://github.com/tpope/vim-surround) that allows replacing, remove or adding things around text objects. The plugin [vim-repeat](https://github.com/tpope/vim-repeat) adds the functionality to repeat an action in a lot of other plugins. His plugin [vim-fugitive](https://github.com/tpope/vim-fugitive) is a fantastic Git wrapper, and [vim-commentary](https://github.com/tpope/vim-commentary) allows to comment out lines or blocks of code.

I use more plugins of Tim Pope, it’s worth checking out all his plugins.

### TMUX

And finally, I use three plugins to make working with Vim in TMUX more enjoyable: [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator) and [Vimux](https://github.com/preservim/vimux). The first plugin allows moving seamlessly between Vim and TMUX, including splits. The second plugin allows interaction with TMUX from inside Vim. There are additional plugins available to run test suites for specific languages.

## Conclusion

Investing in your tools is something every developer should do. It does make you more productive, faster and you enjoy working with these tools more. If you’re not yet a Terminal user, start learning the basics and learn something new every day. Learning Vi/Vim/Neovim is a life-long journey, but it’s rewarding and enjoyable. Once you started using Vim you never want to go back to another editor.

---

This is the second part of a two-part series on Setting Up a Web Developer Work Environment.

1. [Terminal, Zsh, and TMUX](/web-developer-work-environment-1/)
2. **Neovim**

[^stackoverflow2021]: Stack Overflow _Developer Survey 2021_, <https://insights.stackoverflow.com/survey/2021#section-most-loved-dreaded-and-wanted-collaboration-tools>.
