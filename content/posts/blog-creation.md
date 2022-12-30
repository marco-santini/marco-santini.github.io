---
title: "Here we go!"
date: 2022-12-03T16:17:27+01:00
draft: false
tags: ['Github', 'Pages', 'Actions', 'Blogging', 'Hugo','Markdown']
---

<img src="/posts/blog_creation_header_minified.jpg" />

# Why?

Over the years I have always looked for the time to take care of small personal projects. My problem has always been organizing some notes as I progress in my work. Recently I had the need to manage these *"experiments"* for longer periods of time and so, obviously, things got complicated having to rely only on my memory. What better occasion then to change approach and create a personal blog?

Yes but... I wouldn't want to manage a CMS and I don't want even scout for a hosting service... I just want to write some notes... maybe some formatted notes... Yeah! Markdown!

Markdown is very easy to write and, luckily, nowadays there are many tools to convert it into an elegant website.

This project is also an opportunity to make friends with [Hugo](https://gohugo.io) *"The world's fastest website building framework."*. I've been looking at this project for a long time but haven't found the right occasion to try it. Maybe it's an overkilling choice but, as I said, I want to make some experience with this tool.

To deploy this blog I have decided to use [Github Pages](https://pages.github.com)
I'm not going to write a step by step tutorial but I want to "memorize" the first steps I took to publish this blog.

# Prerequisites
 - Github account or other service that offer a similar [Pages](https://pages.github.com) service. (e.g. [Gitlab](https://docs.gitlab.com/ee/user/project/pages/), [Codeberg](https://codeberg.page))
 - git cli


# Hugo install ([docs](https://gohugo.io/installation/))

First of all we need the Hugo cli. Hugo is written in golang and the cli is a single binary. One way to install it is using brew.

```bash
brew install hugo
```


# Init blog

I need a working dir to create site scaffold.

```bash
cd ~/Development
hugo new site myBlog && cd myBlog
```

# Set theme (I've choose [LoveIt](https://hugoloveit.com))

Hugo has tons of [themes](https://themes.gohugo.io) to choose from, just select the one that best suits your style.

```bash
git init
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

# Configuration

The easiest way to start setting up your site is to customize *config.toml*. This approach is fine to get you started, but you can leverage on a variety of [configuration](https://gohugo.io/getting-started/configuration/) model to take control of this tool.

# Create this post

Let's create our first post...

```bash
hugo new posts/blog-creation.md
```

Here we are... now it's time to write some content in simple markdown format in the file *content/posts/blog-creation.md*

# Action!

When I'll post a new *tale* the only thing I want to do is to push a commit on a branch and, thanks to Github actions, this is exactly what I'll have to do.

This is the workflow config file that define blog build action.

```yaml
name: github pages

on:
  push:
    branches:
      - main # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0 # Fetch all history for .GitInfo and .Lastmod
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "latest"
          # extended: true
      - name: Build
        run: HUGO_ENV=production hugo --minify
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

# Some repo config

To create a Github personal Page the repo name must be <username>.github.io (read the doc for other configuration). 

The Deploy step push the built content on an orphan branch named **gh-pages**, so we have to configure in *Settings* section of the repo the source branch that we want to serve.

<img src="/posts/github_config.png" />

> You can change this default behaviour and other params based on actions-gh-pages [docs](https://github.com/marketplace/actions/github-pages-action#%EF%B8%8F-set-another-github-pages-branch-publish_branch)

# And that's it

Today it is very simple to create a cool site without the need to know a programming language or to manage servers. There are many tools to achieve this, all with pros / cons, it's up to you which one best fits your needs.
