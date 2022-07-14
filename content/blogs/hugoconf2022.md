---
title: "How to build a Developer Profile - HugoConf2022"
date: 2022-06-27T22:47:37+05:30
draft: false
type: "post"
ogtype: "article"
tags: ["hugo","gohugo","hugoconf2022","conf","developer","profile"]
slug: "build-developer-profile"
categories: ["Hugo"]
cover:
    image: "/img/misc/hugoconf2022.jpeg"
    alt: "Hugoconf2022 how to build a developer profile"
---

# Just a thought :thought_balloon:  ... 

As a Developer, we keep learning & developing stuff. Sometimes we also find the solutions. Our stuff is distributed the same as our System architecture. We maintain different platforms like GitHub for projects, Medium for blogs, LinkedIn for profile, etc.  

All this stuff we want to share on a single platform but as a tech, I am lazy if you ask me to install some CMS and maintain all stuff over there. it is hard to switch from a black terminal :computer:  window to some UI :sweat_smile:

As a developer I was always thinking can I write my blogs, article or notes in Vim? What if we can publish the blog the same as we do releases by hitting some commands.


In this gist, I am quickly going to give you some suggestions for tools that will help you to build your platform.

# The Requirement :memo:

* Write a blog in any editor (For me its Vim)  
* Easy to publish
* Developer Friendly
* Lightweight and speed
* Cost effective

# Let's Build :wrench:

I was evaluating multiple tools while searching for a platform and found Hugo which is easy, fast, and handy as a developer. Hugo is an opensource static site generator which means you can write everything in Markdown and Hugo will generate the site accordingly. You need to know the basic Markdown syntax and you are good to go.

You can refer official [hugo](https://gohugo.io/documentation/) document to get started.

Below is some tool that helped me to set up my platform.

---

# Theme

Lots of themes are present which you can configure with your Hugo site. There are some common features across the themes and some themes provide the special features also. You can explore all themes here [themes.gohugo.io](https://themes.gohugo.io/)  

I was looking for a simple theme which has a simple layout with menus, a dark theme, and tech friendly.

## PaperMod

I have build my [website](https://ashish.one) in [PaperMod](https://github.com/adityatelange/hugo-PaperMod). Few pointers why i choose:

### 1. [Search](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-features/#search-page) :mag:

PaperMod uses [Fuse.js](https://fusejs.io/getting-started/different-builds.html#explanation-of-different-builds) Basic for search functionality.

### 2. Post Cover Image :tokyo_tower: 

It gives an easy option to add a cover image to your post.

### 3. Edit link for post :pencil2:

`Suggest changes` option to ask viewers to contribute or Raise PR.

You can check more details about all features [here](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-features)

## [hyde-hyde](https://github.com/htr3n/hyde-hyde)

hyde-hyde I found a simple and easy theme if you want to get started with a simple menu and post. I chooses this because of its simplicity and then migrated to PaperMod.

# Comments :speech_balloon:

Once your audience starts reading your article, they would like to give feedback, suggestion and sometime it could be a discussion. You need someplace like `comments` where viewers can add their points to a particular article. The theme does not come with comments, for that you need to integrate the `comments` tool. Below are some suggestions you can explore:

## 1. giscus

[Giscus](https://giscus.app/) comments system powered by [GitHub Discussions](https://docs.github.com/en/discussions). Let visitors leave comments and reactions on your website via GitHub! As soon as your viewers comment on your article, it will create the discussion thread on Github Discussion. You can explore more about giscus on the official site.

## 2. Utterances 

[utterances](https://github.com/utterance/utterances) lightweight comments system built powered by [GitHub issues](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues). It will create an issue per article once anyone comments. All comments will be associated with a particular Github issue.

## 3. DISQUS

[Disqus](https://disqus.com/) is a blog comment hosting paid service for websites and online communities that use a networked platform. It also comes with social integration, social networks, user profiles, profile notifications, etc.

I migrated my comment system from utterances to giscus. As Github discussion is a proper tool for commenting, discussion, etc. Both are lightweight and you can choose accordingly.

# Shortcodes

While developing the site with Hugo, Shortcodes are your friends. Always check if shortcodes are available for popular tools e.g. youtube, github gist, etc. 

# Hosting :cloud:

After the site generation, you need to host your website somewhere.

Found [Github pages](https://pages.github.com/) best place to host your static site. It allows you to manage your website the same way you manage your projects on github. Get started by creating a simple repository and pushing your source directory.

Check out all steps [here](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages)

Once you set up end to end, You can publish your blog or changes by just basic git commands i.e. `git add`, `git commit`, `git push`.

It will give you the same feeling as you are releasing some features for your project.

You can check more [options](https://gohugo.io/hosting-and-deployment/) about hosting and deployment

I have hosted current website on [github.com/ashishtiwari1993](https://github.com/ashishtiwari1993/ashish.one).

## Domain

By default, Github pages assign the domain like `username.github.io` and you can access your site by visiting that subdomain. You can also setup the custom domain.

# Speed :rocket:

Almost 100% Page speed.

[Pagespeed insights](https://pagespeed.web.dev/report?url=https://ashish.one)

---

# tl;dr

:white_check_mark: Platform - Hugo  
:white_check_mark: Theme - PaperMod  
:white_check_mark: Comment - giscus  
:white_check_mark: Hosting - Github Pages  
:white_check_mark: PageSpeed - 100%  
:white_check_mark: Cost - 100% Free 



{{< youtube fS9tndOl_BY >}}
