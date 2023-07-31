# ashish.one
Source code for my personal website.

# Run on local machine

## Install hugo
[https://gohugo.io/getting-started/installing/](https://gohugo.io/getting-started/installing/)

## Clone the repo
```sh
git clone https://github.com/ashishtiwari1993/ashish.one.git
cd ashish.one
```

## Install theme (e.g. PaperMod)

```sh
git clone https://github.com/adityatelange/hugo-PaperMod themes/PaperMod
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

## Run 
```sh
hugo server -D
```

Visit [localhost:1313](http://localhost:1313)

## To enable the comment

Open below file

```sh
vim themes/PaperMod/layouts/partials/comments.html
```

Add below snippet

```sh
<script src="https://giscus.app/client.js"
        data-repo="ashishtiwari1993/ashish.one"
        data-repo-id="MDEwOlJlcG9zaXRvcnkyMTAxMjkzNjY="
        data-category="General"
        data-category-id="DIC_kwDODIZR1s4CP1P2"
        data-mapping="title"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="dark_high_contrast"
        data-lang="en"
        crossorigin="anonymous"
        async>
</script>
```

## Add Cover page customization

Create below file

```sh
vim themes/PaperMod/assets/css/extended/tag_list_cover.css 
```

Add below CSS

```
.tag-entry .entry-cover {
    display: flex;
}
```

Re Generate the Site

```
hugo
```

## Open links in new tab

[https://github.com/adityatelange/hugo-PaperMod/discussions/760#discussioncomment-2021778](https://github.com/adityatelange/hugo-PaperMod/discussions/760#discussioncomment-2021778)
