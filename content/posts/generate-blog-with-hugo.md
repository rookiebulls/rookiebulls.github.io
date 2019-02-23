---
title: 'Generate website with Hugo and deploy it with Travis CI'
date: 2019-02-23T10:37:06+08:00
draft: false
slug: generate-blog-with-hugo
categories:
  - 'Notes'
tags:
  - 'Hugo'
---

## Why Hugo

I used to use [Pelican](https://blog.getpelican.com) to generate my website, it works petty well. But in some way, it's just not that perfect, such as, no hot reload, small community, lots of dependencies, not that fast. Hugo, in other way, is fast, nearly no dependencies, better writing experience with hot reloading, so it's clearly a better choice for me.

## Install Hugo

If you are on Mac OS, you can install Hugo with Homebrew `brew install hugo`, for other platforms, check this [link](https://gohugo.io/getting-started/installing), you can find your answers here.

## Generate website

Use command `hugo new site sitename` to generate a website.

## Install a theme

```bash
cd blog/themes
git clone https://github.com/liuzc/LeaveIt.git
```

## Customize your configuration

Open `config.toml` and set the `theme` property to your theme.

```conf
baseURL = "/"
languageCode = "en-us"
title = "rookiebulls"
theme = "LeaveIt"
...
```

For more configurations, check your theme's example site.

## Add a new post

Run command `hugo new posts/new-post-title.md` to add a new post in the `content/posts` directory. Hugo supports `Markdown` out of box, if you are not familiar with it, here's a [cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) about it.

## Build you website

Just type `hugo`, your website will be generated automatically in the `public` directory.

## Deploy your website

The content in the `pubic` directory is static, you can host it with Nginx, Apache... any web server will be fine, but you need to have a VPS first. I choose Github Pages to host my website, because it's free and perfectly suitable for static site.

1. Create a new repository
   ![](https://ws4.sinaimg.cn/large/006tKfTcgy1g0gabrlfurj31dz0u07b9.jpg)
2. Push your content to master branch
   ![](https://ws2.sinaimg.cn/large/006tKfTcgy1g0gaf36divj314l0u0to5.jpg)

## Auto deploy your website with Travis CI

Doing build, copy, paste and push operations all the time is really annoying, so I decide to do it in an automatic way. Travis CI is a perfect solution for this, it works perfectly with Github, you just need to push your source content to Github, and let Travis CI do the rest.

1. Sign up with your Github account.
   ![](https://ws1.sinaimg.cn/large/006tKfTcgy1g0garjc1wcj31o20u0dlr.jpg)
2. Add your repository
   ![](https://ws4.sinaimg.cn/large/006tKfTcgy1g0gay5l3zkj31se0pejvc.jpg)
   ![](https://ws2.sinaimg.cn/large/006tKfTcgy1g0gb01jd37j318g0nsad2.jpg)
3. Create a new personal access token
   ![](https://ws4.sinaimg.cn/large/006tKfTcgy1g0gb3t3fy1j31ns0h443a.jpg)
4. Set up your repository settings in Travis, add a new environment variable named GITHUB_TOKEN with the token you created in step 3
   ![](https://ws3.sinaimg.cn/large/006tKfTcgy1g0gb6dbo4yj31q10u0gsn.jpg)
5. Add .travis.yml to your website root directory

```yaml
language: go

go:
  - '1.11' # Use Golang 1.11

# Specify which branches to build using a safelist
branches:
  only:
    - source

install:
  # install hugo
  - go get github.com/gohugoio/hugo

script:
  # build website
  - hugo

deploy:
  provider: pages # Important, it means using github pages to deploy
  skip-cleanup: true # Important, keep it to true
  local-dir: public # Your content direcotry after building
  target-branch: master # Which branch to push to
  github-token: $GITHUB_TOKEN # Your github access token for travis
  keep-history: true # keep the target-branch history
  on:
    branch: source # source content branch
```

6. Push your source content to `source` branch

With all this done, next time your update your source content, Travis CI will deploy it automatically.
![](https://ws1.sinaimg.cn/large/006tKfTcgy1g0gbm1z0psj322c0bomzp.jpg)
