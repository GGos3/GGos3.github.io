---
title: hexo 블로그 github action 설정
date: 2022-10-07 18:10:46
tags:
categories: "hexo"
---

## github action이란?

github에서 제공하는 CI/CD 서비스로 코드 저장소에 어떤 이벤트가 발생 했을 때 특정 작업을 실행 시키거나 주기적으로 작업을 실행시킬 수 있는 서비스다.

## github action 설정

프로젝트 내에 `.github\workflows\gh-pages.yaml` 파일을 생성해 주고

아래의 내용을 작성 후 push 해준다.

`gh-pages.yaml`

```yaml
name: Pages

on:
  push:
    branches:
      - main # default branch

jobs:
  pages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js 16.x
        uses: actions/setup-node@v2
        with:
          node-version: "16"
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Theme Dependencies
        run: git submodule update --init --recursive
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

프로젝트의 코드 저장소에서 아래 사진을 따라 설정 한다.

![Untitled](/source/images/github_action1.png)

이제 `hexo new` 로 글 작성 후 push 하면 자동으로 페이지에 추가 되어 있다.

---

## 과정 정리

![Untitled](/source/images/Github_action2.png)

이벤트가 발생하면 `github action`이 `hexo generator` 를 해주고

이로 인해 생성된 `public` 폴더 내에 있는 파일들을 `gh-pages` branch로 옮겨 서비스 한다.
