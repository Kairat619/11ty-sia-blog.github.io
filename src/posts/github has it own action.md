---
title: github has it own action to deploy page
description: now github has it own action to deploy page, here an updated pipeline
date: 2024-04-16
tags:
  - 11ty
layout: layouts/post.njk
---
'''name: build 11ty site

on:
  push:
    branches: ["main"]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:

  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - name: Install dependencies & build
        run: |
          npm install
          npx @11ty/eleventy
      - uses: actions/upload-pages-artifact@v2

  deploy:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - id: deployment
        uses: actions/deploy-pages@v2
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

'''

just make sure the output dir is _site

