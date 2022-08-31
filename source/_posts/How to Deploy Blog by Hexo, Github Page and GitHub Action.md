---
title: How to Deploy Blog by Hexo, Github Page and GitHub Action
date: 2022-02-11 09:00:00
categories: Toolkit
---

I have recently deployed my personal blog via Hexo, GitHub Page and GitHub Action, which supports continuous deployment when pushing code modifications to GitHub. It's free, convenient and, most importantly, enjoyable to set up! Let's dive in!

### Set up Hexo Locally

- Download and implement Node.js on the laptop, in which I use the operating system of Windows.
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
