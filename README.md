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
<script src="https://utteranc.es/client.js"
        repo="ashishtiwari1993/ashish.one"
        issue-term="title"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>
```

Re Generate the Site

```
hugo
```
