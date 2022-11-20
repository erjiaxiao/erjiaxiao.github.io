---
title: How to Deploy a Blog by Hexo and Github
date: 2022-01-23 09:00:00
categories: Toolkit
index_img: /img/github_page.jpg
excerpt: I have recently deployed my personal blog via Hexo, GitHub Page and GitHub Action, which supports continuous deployment when pushing code to GitHub.
---

## Introduction

I have recently deployed my personal blog via Hexo, GitHub Page and GitHub Action, which supports continuous deployment when pushing code to GitHub. It's free, convenient and, most importantly, enjoyable to set up! Let's dive in!

![Github](/img/github_page.jpg)

## Set up Hexo Locally

- Download and implement [Node.js](https://nodejs.org/en/download/) on the laptop, in which I use the operating system of Windows.
- Execute the following bash in the command line to check if Node.js is installed correctly and install the hexo toolkit.
  ```bash
  node -v
  npm -v
  npm install -g hexo-cli
  ```
- Initialize Hexo blog locally and set it up, which could be visited on http://localhost:4000

  ```bash
  mkdir myblog
  hexo init myblog
  cd myblog
  npm install
  hexo s
  ```

## Prepare the Repository

Create a repository named `your_user_name.github.io` on Github and create two branches named `master` and `hexo`. Note that `your_user_name` here must be the same as your Github account.

## Add the Workflow File

Add a file `myblog/.github/workflows/deployment` to make continuous deployment via GitHub Action.

```yaml
 name: Deployment

 on:
 push:
     branches: [hexo] # only push events on source branch trigger deployment

 jobs:
 hexo-deployment:
     runs-on: ubuntu-latest
     env:
     TZ: Asia/Shanghai

     steps:
     - name: Checkout source
     uses: actions/checkout@v2
     with:
         submodules: true

     - name: Setup Node.js
     uses: actions/setup-node@v1
     with:
         node-version: '12.x'

     - name: Install dependencies & Generate static files
     run: |
         node -v
         npm i -g hexo-cli
         npm i
         hexo clean
         hexo g

     - name: Deploy to Github Pages
     env:
         GIT_NAME: your_user_name
         GIT_EMAIL: your_email
         REPO: github.com/your_user_name/your_user_name.github.io
         GH_TOKEN: ${{ secrets.GH_TOKEN }}    # can be created or changed in the Github/Repository/Setting/Secrets/Actions
     run: |
         cd ./public && echo cloudwind.tech > CNAME && git init && git add .
         git config --global user.name $GIT_NAME
         git config --global user.email $GIT_EMAIL
         git commit -m "Site deployed by GitHub Actions"
         git push --force --quiet "https://$GH_TOKEN@$REPO" master:master
```

## Write Posts and Push Code

- Write Posts locally and save them in `myblog/source/_posts`.
- Push the directory `myblog` to the `hexo` branch. Note that Since we have set the workflow, whenever we push code, Github Action will help us compile and save all the generated static files in the `master` branch, which saves us tons of time spent on manually deploying the blog.
- We could also optionally install [blog themes](https://hexo.io/themes/). Now the blog can be visited on the default domain `https://your_user_name.github.io`, which could be customized to our own domain in the repository setting on Github.
- Enjoy!
