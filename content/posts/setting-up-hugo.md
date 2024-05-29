---
title: Setting Up Hugo
date: 2024-05-24T09:15:55+01:00
draft: false
tags: [Hugo,IndieWeb]
---
This site it built with [Hugo](https://gohugo.io/), which is a static site generator. That means that I write the content as [markdown](https://www.markdownguide.org/) and then use the hugo tool to generate the html etc. that forms this website. I can then copy the html and theme stylesheets to my webhost (using rsync). This is the same approach I took with my [other website](https://www.vurt.co.uk).

One issue I had with my old website was that I was limited to updating the site from my laptop - I couldn't easily fire off a quick note or post from my phone or elsewhere. Even though I had the site content stroed in github, and I could have setup hugo on other devices it was always a bit of extra friction which allowed my laziness to kick in.

This time round, I'm making use of [github actions](https://github.com/features/actions) to handle the site generation and deployment. I've created a workflow, which is stored in the git repo for this site. It checks out the code, runs hugo on it and then uses rsync to deploy the site content to my webhost. The workflow gets triggered whenever I push a commit to github.

Nicolas Martignoni has a [great write up](https://blog.martignoni.net/2022/07/deploy-hugo/) of this approach and I basically copied what is written there.

Here's my workflow:

```yaml
name: Hugo CI & deploy

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    name: Build and deploy website
    runs-on: ubuntu-latest
    steps:
     - name: Checkout
       uses: actions/checkout@v4
       with:
         submodules: true
     - name: Setup Hugo
       uses: peaceiris/actions-hugo@v3
       with:
         hugo-version: 'latest'
         extended: true
     - name: Cache Dependencies
       uses: actions/cache@v4
       with:
         path: /home/runner/.cache/hugo_cache
         key: ${{ runner.os }}-hugomod-${{ hashFiles('**/go.sum') }}
         restore-keys: |
           ${{ runner.os }}-hugomod-
     - name: Build website with Hugo
       run: hugo --minify
     - name: Deploy website with rsync
       uses: burnett01/rsync-deployments@7.0.1
       with:
         switches: -avzr --quiet --delete
         path: public/
         remote_path: ${{ secrets.DEPLOY_DIRECTORY }}
         remote_host: ${{ secrets.DEPLOY_HOST }}
         remote_user: ${{ secrets.DEPLOY_USER }}
         remote_key: ${{ secrets.DEPLOY_KEY }}
```
The secrets are setup in github and store the details of the ssh key and where to rsync the files to.

Overall, this removes one area of friction - now I can deploy site changes from anywhere. However, creating and editing posts still requires me to check out my repo and have hugo locally installed. To get around that, I'll next be making use of a locally hosted [indiekit server](https://getindiekit.com/). Hopefully my next update will have more details of that.
