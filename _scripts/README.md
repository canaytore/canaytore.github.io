I was inspired by [David Robinson's blog](http://varianceexplained.org/), where he compiles his blog posts with [knitr](http://yihui.name/knitr/) and R markdown using [this script](https://github.com/canaytore/canaytore.github.io/blob/master/_scripts/knitpages.R).

There are a number of files/sub-directories within this repository that are used for formatting and rendering my blog. The remaining sub-directories/scripts I've created and modified for my own purpose. This repository has the following main components:

- `_R`: contains raw .Rmd scripts containing source code and text for individual blog posts
- `_posts`: houses the rendered .Rmd scripts (from `_R`); these are .md files
- `_scripts`: currently houses a script used to knit non-rendered .Rmd files in `_R` 
- `assets`: miscellaneous blog images and resources
- `figs`: location of images and figures rendered from .Rmd posts
