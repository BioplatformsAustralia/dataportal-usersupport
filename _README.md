# Bioplatforms Data Portal User Support Guides

## Content

User support articles for the Bioplatforms Data Portal.

1. [How to register for an account and log-in](registration_login.md)
2. [How to find, filter and bulk download data and meta-data](find_filter_download.md)
3. [How to filter Australian Microbiome data](metadata_filter_ausmicrobiome.md)
4. [Programmatic access to the Bioplatforms Data Portal](programmatic_access.md)

For the production view for end users linked off the BPA Data Portal, this 
uses a Jekyll remote theme that is compatible with Github Pages.

For development/authoring of content, a local build is possible with Jekyll,
or a simplified view with RMarkdown/RStudio

## Jekyll 

### Remote theme

There is a Jekyll theme that matches the main Bioplatforms Australia Data 
Portal website.  It is located in the following repository.

https://github.com/BioplatformsAustralia/jekyll-theme-bpa

Remote themes for GitHub Pages are discussed here:

https://github.blog/2017-11-29-use-any-theme-with-github-pages/

This was used as a reference implementation during the process
of getting it working:

https://github.com/MichaelCurrin/jekyll-theme-quickstart

### Navigation

The navigation for the site is built based on the contents of ```_config.yml```

If further User Support articles are added, they must be included here.


### Local build using Jekyll

The site can be built locally for development and testing.

A local ruby install is required.  Use of ```rbenv``` to setup a development
environment, with the ```bundler``` gem needed to install Jekyll and
associated plugins.

A Makefile is provided with the following targets:

* _install_ - install the necessary gems
* _serve_ - run a local web server to view the content
* _build_ - build a static HTML render of the content

The build uses same remote theme as the production render.  Render 
output will end up in the ```_site``` directory

### Production render on Github Pages

The Remote Theme is specified in ```_config.yml``` file

The repository for the theme must have public access enable on Github.

The site is configured on the Github Pages settings page at:

https://github.com/BioplatformsAustralia/dataportal-usersupport/settings/pages

The Source is set on the master branch, with root specified as the relevant
directory.  No theme is chosen here as it is picked up from the ```_config.yml```
file.

## RMarkdown

### Navigation

### Local build using RStudio / RMarkdown

---

---

## Attributions for the Jekyll render

## Attributions for building the RMarkdown render

- Rmarkdown
     - [Cookbook](https://bookdown.org/yihui/rmarkdown-cookbook/)
     - [Site](https://bookdown.org/yihui/rmarkdown/rmarkdown-site.html)
     - [Appearance & style](https://bookdown.org/yihui/rmarkdown/html-document.html#appearance-and-style)
     - [Rstudio lesson for rmarkdown: lesson 13](https://rmarkdown.rstudio.com/lesson-13.html)
     - [HTML-CSS](https://bookdown.org/yihui/rmarkdown-cookbook/html-css.html)
- [GitHub & Rstudio whitepaper](https://resources.github.com/whitepapers/github-and-rstudio/)
- [Image hyperlinks](https://stackoverflow.com/a/42235353)
- [Google Analytics](https://stackoverflow.com/a/45169407)
- [Prevent body / navbar overlap](https://stackoverflow.com/a/41979464)
- [Side nav bar](https://www.w3schools.com/howto/howto_js_sidenav.asp)
- [CSS website layout](https://www.w3schools.com/css/css_website_layout.asp)
- [CSS page width](https://stackoverflow.com/a/46564977)
- [Add elements to header - including favicon](https://bookdown.org/yihui/rmarkdown-cookbook/favicon.html)

Xie Yihui, Dervieux Christophe, Riederer Emily. 2020. R Markdown Cookbook. [https://bookdown.org/yihui/rmarkdown-cookbook/](https://bookdown.org/yihui/rmarkdown-cookbook/)

Xie Yihui, Allaire J. J., Grolemund Garrett. 2020. R Markdown: The Definitive Guide. [https://bookdown.org/yihui/rmarkdown/](https://bookdown.org/yihui/rmarkdown/)

R Core Team (2020). R: A language and environment for statistical computing. R Foundation for Statistical Computing,
  Vienna, Austria. URL https://www.R-project.org/.
