---
title: "Add Responsive Google Slides on Hugo"
date: 2020-04-13T14:11:53+05:30
draft: false
ogtype: "article"
type: "post"
tags: ["hugo","shortcode","google slides","gslides","responsive"]
slug: "add-responsive-google-slides-on-hugo"
categories: ["Hugo"]
---

Steps to add Responsive google slides iframe with Hugo:

**Hugo version:**

```sh
$ hugo version

Hugo Static Site Generator v0.69.0-4205844B linux/amd64 BuildDate: 2020-04-10T09:12:34Z
```

## Step 1: Create Shortcode

### Create `gslides.html` file

```sh
vim layouts/shortcodes/gslides.html
```

## Step 2: Add below code:


```html
<div id="Container"
 style="padding-bottom:56.25%; position:relative; display:block; width: 100%">
 <iframe id="googleSlideIframe"
  width="100%" height="100%"
  src="{{ .Get "src" }}"
  frameborder="0" allowfullscreen=""
  style="position:absolute; top:0; left: 0"></iframe>
</div>
```

## Step 3: Use shortcode 'gslides' in your Blog/Post Markdown file

Simply place below snippet in your markdown file. 

Replace `src` value with your Google slide URL.

```html
{{< exampleGslide >}}
```


> **I have shown for google slides, Similarly, you can create or add any other document type like google sheets, doc, etc. OR you can place any iframe like this.**
