# Template structure

## Pages
1. [Storefront theme](./../)
2. [Template structure](./../structure/)
3. [JavaScript methods API](./../methods/)
4. [Implement a search engine](./../search/)

## Summary
1. [Introduction](#introduction)
2. [Basic directory tree](#basic-directory-tree)
3. [PWA](#pwa)
4. [Complete example](#complete-example)

{% raw %}

# Introduction
Such as any other static web application, you are free to
set up your own directory structure to your E-Com Plus store template,
it should work as storefront.

This document is only intended to define few files and directories names,
to make the template fully compatible with
<a href="https://github.com/ecomclub/dynamic-backend" target="_blank">dynamic backend</a> and
E-Com Plus dashboard tools.

Before you start building your template,
you must also read (at least) the storefront
<a href="./../">theme specifications</a>.

# Basic directory tree
```
├── .builder
│   ├── html
│   ├── plugins
│   └── scss
├── .dist
├── .rendered
└── static
    ├── css
    ├── fonts
    ├── img
    │   └── icons
    └── js
```

Inside the `static` folder should be all non-HTML files,
you can also create custom folders within it.

The `.rendered` and `.dist` folders
should be **left empty**.

In almost all cases, all HTML files should be
inside the root directory, although you can create custom
folders if necessary.

# Predefined files
```
├── index.html
├── _brands.html
├── _categories.html
├── _collections.html
└── _products.html
```

The **above files are required**, with the specified names.
They have to be in the root directory.

To complete the storefront app,
you should also create other HTML files, eg.:

```
├── search.html
├── user.html
└── cart.html
```

It's possible to use as many HTML files as you want,
and you can choose any filenames.

# PWA
If you want to build a
<a href="https://developers.google.com/web/progressive-web-apps/" target="_blank">Progressive Web App</a>
(recommended),
you should put `service-worker.js` on root directory (as needed)
and `manifest.json` inside `static` folder:

```
├── service-worker.js
└── static
    └── manifest.json
```

# Complete example
```
├── .builder
│   ├── html
│   ├── plugins
│   └── scss
├── .dist
├── .rendered
├── _brands.html
├── _categories.html
├── _collections.html
├── _products.html
├── cart.html
├── index.html
├── search.html
├── service-worker.js
├── static
│   ├── css
│   │   └── app.css
│   ├── fonts
│   ├── img
│   │   ├── banner.png
│   │   └── icons
│   │       └── favicon.png
│   ├── js
│   │   └── app.js
│   └── manifest.json
└── user.html
```

{% endraw %}
