---
title: How to Deploy Blog by Hexo, Github Page and GitHub Action
date: 2022-01-23 09:00:00
categories: Toolkit
---

I have recently deployed my personal blog via Hexo, GitHub Page and GitHub Action, which supports continuous deployment when pushing code modifications to GitHub. It's free, convenient and, most importantly, enjoyable to set up! Let's dive in!

### Set up Hexo Locally

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

### Prepare a Repository and Set the Workflow

- Create a repository named `your_user_name.github.io` on Github and create two branches named `master` and `hexo`. Note that `your_user_name` here must be the same as your Github account.
- Add a file `myblog/.github/workflows/deployment` to make continuous deployment.

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
           GH_TOKEN: ${{ secrets.GH_TOKEN }}    # Generate it in the Github setting for access permission
       run: |
           cd ./public && echo cloudwind.tech > CNAME && git init && git add .
           git config --global user.name $GIT_NAME
           git config --global user.email $GIT_EMAIL
           git commit -m "Site deployed by GitHub Actions"
           git push --force --quiet "https://$GH_TOKEN@$REPO" master:master
  ```

- Push the directory `myblog` to the `hexo` branch. Note that Since we have set the workflow for Github Action, each time we push code to the `hexo` branch. Github will help us compile them and save all the generated static files in the `master` branch, which saves us tons of time spent on manually deploying the blog.

### Almost Done

We could optionally install [blog themes](https://hexo.io/themes/) and write interesting posts locally. Just a click to push code to Github and wait for automatic deployment. Finally, we could also set our custom domain in Github instead of the default domain `https://your_user_name.github.io`.

### Enjoy!
