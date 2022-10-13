---
title: hexo 블로그 만들기
date: 2022-10-07 09:00:03
tags: hexo blog
categories: "hexo"
comments: true
---

# Hexo 설치

## 초기 세팅

1.`npm`을 이용해 hexo를 설치

```bash
$ npm install hexo-cli -g
```

만약 Node Js가 설치 되어있지 않다면 설치가 필요하다.
https://nodejs.org/en/

2.원하는 디렉토리로 이동 후 hexo로 blog 프로젝트를 생성한다.

```bash
$ hexo init blog
$ cd blog
$ npm install
```

3.터미널에 아래 명령어를 실행해 hexo가 설치 되었는지 확인한다.

```bash
$ hexo s
```

정상적으로 구동 되었다면 터미널에 제공된 `https://localhost:4000/`으로 접속해 본다.

---

## oranges 테마 적용

1.git 명령어 사용을 위한 repo를 생성한다

```bash
$ git init
```

2.oranges 테마를 설치한다

```git
git submodule add https://github.com/zchengsite/hexo-theme-oranges.git themes/oranges
```

3.테마가 잘 적용 되었는지 확인한다.

```bash
$ hexo s
```

---

## oranges 테마 수정

테마를 수정할 때에는 `_config.oranges.yml`이라는 파일을 새로 생성해 작성해야한다.
변경 사항만 작성해도 되지만 편의를 위해 `themes/oranges/_config.yml`파일에 있는 내용을 복사 했다.
테마에 수정할 곳이 있다면 https://github.com/zchengsite/hexo-theme-oranges 해당 링크에서 찾아 수정하면 된다.
예시로 코드 블럭의 스타일을 변경 해보겠다.

`_config.oranges.yml`

```yml
# 代码块配置
codeBlock:
  # 代码块风格，默认normal
  # 可选：默认风格'normal'；mac黑色风格：'mac-black'
  style: "normal" <-- 이 부분을 'mac-black' 으로 바꿔준다
  # 代码复制
  copy:
    enable: true

```

---

# 블로그 글 작성

1. 터미널에 아래의 명령어를 실행한다.

```
$ hexo new "포스트명"
```

2. `/source/_posts/`경로에 생성된 `포스트명.md`파일을 MarkDown 문법에 맞게 작성한다.

`포스트명.md`

```markdown
# 야호 첫 글이에요!

[x] 글 작성하기
```

이상 포스팅을 마치겠다.
