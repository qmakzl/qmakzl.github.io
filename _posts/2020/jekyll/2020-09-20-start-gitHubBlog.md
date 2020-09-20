 ---
title: GitHub 블로그 시작하기
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- Jekyll
toc: true
toc_sticky: true
toc_label: 목차
description: 깃 블로그 하는 방법을 기술하고 깃 블로그란 무엇인지  GitBlog와 Jekyll란 무엇인지 또한  로컬 개발 환경을 위한 루비설치를 어떻게 하는지 _config.yml 세팅 등 깃 블로그를 처음 시작할때 겪었던 어려움을 극복하는 페이지
article_tag1: GitHub(GitBlog) 블로그
article_tag2: minimal-mistakes
article_tag3: Jekyll
article_section: 깃 블로그 따라하기
meta_keywords: 깃블로그,GitBlog,GitHub블로그,minimal-mistakes,Jekyll
last_modified_at: 2020-01-23T00:00:00+08:00
---

기존에 하던 깃 블로그에서 모바일 구간 깨지는 이슈를 수정하고 디자인적인 아쉬움을 보완하고자  하였습니다.
또한 GitBlog 새로운 테마를 세팅하면서 겪었던 어려움을 공유하고 누구나 바로 Github 블로그에 자신의 글을 쉽게 올릴 수 있도록 글을 공유합니다.

## Step 1:  깃 블로그란
깃 블로그란  Github 저장소에 저장된 html 파일과 같은 정적 웹 문서들을 GitHub에서 무료로 웹에서 볼 수 있도록 호스팅 서비스를 제공해 주는 것입니다. 
때문에 Github을 이용하는 사용자들은 **누구나 고유의 정적 웹 사이트 1개를 가질 수 있습니다.** 계정이 없다면 [Github]({{ "https://github.com/" }}){:target="_blank"} 에서 Github계정 생성합니다.
계정을 만들고 신규 Repository를 **{Git ID}.github.com** 으로 세팅합니다. 

**Please Note:** 해당 포스트는 어느 정도 깃의 사용법을 알고 있다는 가정하에 작성하였습니다.
{: .notice--danger}

![Git repository 신규 생성 이미지]({{ site.url }}{{ site.baseurl }}/assets/images/post/jekyll/create-repository.png){: .align-center}

정상적으로 생성되었다면 세팅 메뉴 중 하단 GitHub Pages가 그림과 같이 활성화되어있을 것입니다.
<br /> https://github.com/{ Git ID }/{ Repository 이름 }/settings <br />

![Git repository Setting 확인]({{ site.url }}{{ site.baseurl }}/assets/images/post/jekyll/settings-1.png){: .align-center}

![Git repository Setting 확인]({{ site.url }}{{ site.baseurl }}/assets/images/post/jekyll/settings-2.png){: .align-center}

위 그림과 같이 정상적으로 반영 되었다면 https://{ Git ID }.github.io/ URL 접근 가능합니다.

## Step 2: GitBlog와 Jekyll

이러한 Github page에 Jekyll을 결합한다면 좀 더 생산적이고 강력한 블로그를 만들 수 있습니다. Jekyll이란 HTML(.html), Markdown(.md) 등 다양한 포맷의 텍스트들을 읽고 가공하여 자신의 웹 사이트에 바로 게시할 수 있게 해주는 Rubby언어로 만들어진 하나의 **텍스트 변환 엔진**이라고 보면 됩니다. 쉽게 말해 html을 모르더라도 공부하기 비교적 수월한 markdown 파일을 작성하면 알아서 html파일로 변환되어 웹 서비스를 구축해 준다고 생각하면 됩니다. 지킬을 사용하여 게시글을 작성한다면 웹 사이트를 효율적으로 구성할 수 있습니다. Jekyll은 Github의 내부 엔진이기 때문에 Github page에서도 자연스럽게 동작합니다. 감사하게도 이러한 jekyll을 가지고 사용자들이 다양한 테마를 미리 만들고 공유하여 디자인에 대해 깊은 고민은 하지 않게 해주었습니다. 주소는 아래와 같습니다.<br>
<br>
[http://jekyllthemes.org/]({{"http://jekyllthemes.org/"}}){:target="_blank"}<br>
[http://themes.jekyllrc.org/]({{"http://themes.jekyllrc.org/"}}){:target="_blank"}<br>
[https://jekyllthemes.io/]({{"https://jekyllthemes.io/"}}){:target="_blank"}<br>
<br>

![Fork확인]({{ site.url }}{{ site.baseurl }}/assets/images/post/jekyll/fork.png){: .align-center}

다운로드한 minimal-mistakes테마 Zip파일을 Step1에서 생성한 Repository하위에서 압축 해제합니다.


## Step 3: 로컬 개발 환경을 위한 루비설치

Jekyll은 하나의 동적 객체 지향 스크립트 프로그래밍 언어인 Ruby로 작성되었기 때문에 로컬 개발 환경 세팅을 위해서는 Rubby 설치가 필요합니다. 필자는[ https://rubyinstaller.org/downloads/ ]({{" https://rubyinstaller.org/downloads/ "}}){:target="_blank"}해당 사이트에서 2.5.7 버전으로 받아 설치하였습니다. 이제 minimal-mistakes테마를 다운받고 압축해제 하였던 폴더 위치로 이동합니다. 이 후 cmd창을 열어 아래 명령어를 차례로 실행합니다. 꼭 **Gemfile**이 있는 위치에서 실행하여야 합니다.

```java

# gem install bundler
# bundle 
# jekyll serve

```

![Ruby Setting 확인]({{ site.url }}{{ site.baseurl }}/assets/images/post/jekyll/console.png){: .align-center}

정상적으로 설치가 되었다면 이제 [http://localhost:4000/ ]({{"http://localhost:4000/"}}){:target="_blank"}으로 해당 테마를 확인할 수 있습니다.

![로컬서버]({{ site.url }}{{ site.baseurl }}/assets/images/post/jekyll/start-mini.png){: .align-center}

**Please Note:** Rubby 버전에 따라 안되는 케이스가 발생하여 2.5.7로 진행하였습니다. 로컬 서버 포트를 4000에서 다른것으로 바꾸고 싶다면 # jekyll serve --port {원하는 포트 번호} 명령어로 서버를 실행하면 됩니다.
{: .notice--danger}

**Tip:** 기존 로비가 설치되어있고 플러그인 의존성 이슈가 발생하여 특정 버전은 안된다고 나온다면 아래와 같은 명령어로 전체 삭제 후 플러그인 재설치를 해주면 됩니다. <br>
**# gem uninstall -aIx ( 설치된 플러그인 전체 삭제 명령어 )**<br>
**# gem uninstall { 충돌한 플러그인 명 } ( 특정 플러그인 삭제 명령어  )**<br>
{: .notice--info}

이제 Git에 push를 진행하기 전에 압축 해제한 파일 중 불필요한 파일들을 삭제하겠습니다. 
```
.github
test
.editorconfig
.gitattributes
.travis.yml
CHANGELOG.md
docs( 샘플 파일들이 들어가 있는  폴더로 우선 나중을 위해 다른 곳으로 옮겨줍니다. )
```
push를 진행하면 이제 https://{GitID}.github.io 주소로 같은 화면을 볼 수 있습니다. 

## Step 4: Sample 게시물 확인
이전에 이동했덩 docs폴더안 _post폴더를  우리 _post에 덮어 씌운 후  jekyll 서버를 재시작 합니다.  그렇다면 이제   [http://localhost:4000/ ]({{"http://localhost:4000/"}}){:target="_blank"}에서  블로그 게시물 디자인에 참고할 만한  다양한 샘플 포스트를 확인할 수 있습니다.
![Ruby Setting 확인]({{ site.url }}{{ site.baseurl }}/assets/images/post/jekyll/sample-post.png){: .align-center}

블로그 게시물에 대한 네이밍 규칙은 YEAR-MONTH-DAY-title.md입니다. 추후 _post폴더 아래에 게시물을 작성할 때 해당 형식을 지켜 작성해야합니다. 

**Please Note** <br>
필자는 블로그 페이지 작성이 목적이라  .md파일 작성법 및  Jekyll에 대해 깊게 포스팅 하지 않겠습니다.  .yml로 된 파일이나  .md 같은 markdown언어에 대해서 공부하고 싶을 경우를 대비하여 공식 사이트 주소는 남겨드리겠습니다. <br> 
YAML :  [ https://yaml.org/ ]({{" https://yaml.org/"}}){:target="_blank"} <br>
Liquid 문법 : [https://shopify.github.io/liquid/ ]({{"https://shopify.github.io/liquid/"}}){:target="_blank"} <br>
Jekyll 폴더구조 :  [https://jekyllrb-ko.github.io/docs/structure/ ]({{"https://jekyllrb-ko.github.io/docs/structure/"}}){:target="_blank"} <br>
{: .notice--info}

 
```java

# gem install bundler
# bundle 
# jekyll serve

```

## Step 5: 프로젝트 세팅
### _config.yml 수정

지킬 테마에서 자신의 블로그 페이지에 맞게 커스텀 하기위해 _config.yml 을 수정하였습니다.  웹에대한 기본 지식이 있다면 어디를 수정하면 어디가 반영 될 지 직관적으로 알 수 있게 되어있습니다.  꼭 수정해 주어야 하는 부분만 포스팅 하고 이 외 수정한 부분은 아래 주소에서 확인하시기 바랍니다.
[https://github.com/7271kim/7271kim.github.com/blob/master/_config.yml]({{"https://github.com/7271kim/7271kim.github.com/blob/master/_config.yml"}}){:target="_blank"} 

```java


minimal_mistakes_skin    : "mint" # 태마 색 설정 "air", "aqua", "contrast", "dark", "dirt", "neon", "mint", "plum", "sunrise" 

# Site Settings
locale                   : "ko-KR"
title                    : "JAVA Blog" # Meta 태그에 들어가는 영역 , masthead_title등이 없으면 기본값으로 나온다.
title_separator          : "&#124;" # 타이틀 사이 구분자 <title>Welcome to Jekyll | Minimal Mistakes</title> 해당 형식으로 들어갑니다.
subtitle                 : "Version 1.0" # 타이틀 아래에 나올 작은 글씨 
name                     : "이윤기" # 맨 하단 이름 찍히는 영역
description              : "JAVA와 여러가지 개발을 공부하는 블로그" # Meta 태그에 들어가는 영역 
url                      : "https://qmakzl.github.io/" # GitBlog 호스트 주소
baseurl                  : # subPath https://qmakzl.github.io/blog라고 하고 싶을 시 "/blog" 라고 적는다.
repository               : "qmakzl/qmakzl.github.com" # GitHub username/repo-name  
teaser                   : # "/assets/images/senior-couple-4723737_640.jpg" # 홈페이지 기본 티져 이미지
logo                     : # 타이틀 옆에 작게 들어갈 이미지.
masthead_title           : "기록하는 개발자 Blog" # 홈페이지 최 상탄에 들어갈 타이틀
breadcrumbs              : true # 브래드크럼 사용 여부 true, false (default) , _data/ui-text.yml에 다국어 지원 가능합니다.
words_per_minute         : 200 # 해당 포스트 읽는데 걸리는 시간을 계산하기 위해 대락 분당 읽는 수가 몇글자가 될지 적는 공간.

# Site Author
author:
  name             : "이윤기" 
  avatar           : "/assets/images/profile/poto.jpg" # 프로필 이미지
  bio              : "꾸준히 공부하는 개발자입니다. <br> 블로그 포스트 글에서 잘못된 부분이나 수정했으면 하는 부분, 적극적으로 댓글 남겨주신다면 감사하겠습니다."
  location         : "Republic of Korea"
  email            : "qmakzl@naver.com"
  links:
    - label: "Email"
      icon: "fas fa-fw fa-envelope-square"
      #url: mailto:qmakzl1@naver.com
    - label: "Website"
      icon: "fas fa-fw fa-link"
      url: "http://qmakzl.github.com/"
    - label: "Twitter"
      icon: "fab fa-fw fa-twitter-square"
      # url: "https://twitter.com/"
    - label: "Facebook"
      icon: "fab fa-fw fa-facebook-square"
      #url: "https://www.facebook.com/"
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/qmakzl"
    - label: "Instagram"
      icon: "fab fa-fw fa-instagram"
      url: "https://instagram.com/yungi_baby"

# Site Footer
footer:
  links:
    - label: "Email"
      icon: "fas fa-fw fa-envelope-square"
      url: mailto:qmakzl1@naver.com
    - label: "Twitter"
      icon: "fab fa-fw fa-twitter-square"
      # url:
    - label: "Facebook"
      icon: "fab fa-fw fa-facebook-square"
      # url: "https://www.facebook.com/"
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/qmakzl"
    - label: "GitLab"
      icon: "fab fa-fw fa-gitlab"
      # url:
    - label: "Bitbucket"
      icon: "fab fa-fw fa-bitbucket"
      # url:
    - label: "Instagram"
      icon: "fab fa-fw fa-instagram"
      url: "https://instagram.com/yungi_baby"

# Defaults Post들에 적용될 기본 설정들
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: true
      comments: true
      share: true
      related: true
  
  # _pages
  - scope:
      path: "_pages"
      type: pages
    values:
      layout: single
      author_profile: true

```

**Tip:** config.yml  설정에 대한 공식 사이트<br>
[https://mmistakes.github.io/minimal-mistakes/docs/configuration/ ]({{"https://mmistakes.github.io/minimal-mistakes/docs/configuration/"}}){:target="_blank"} 
{: .notice--info}


### navigation 설정
기본 파일은 상단 네비게이션 설정이 되어있지 않습니다. _data/navigation.yml, _config.yml 파일, _pages를 수정하여  Categories, Tag, About이 노출되도록 해보겠습니다.

#### _data/navigation.yml 수정
원하는 네비게이션 url을 설정해 줍니다. http://naver.com과 같이 상대경로가 아닌 절대경로도 가능합니다. 
[https://github.com/7271kim/7271kim.github.com/blob/master/_data/navigation.yml]({{"https://github.com/7271kim/7271kim.github.com/blob/master/_data/navigation.yml"}}){:target="_blank"}

```java
main:
  - title: "Categories"
    url: /categories/
  - title: "Tags"
    url: /tags/
  - title: "About"
    url: /about/
  - title: "연도별 포스트"
    url: /posts/
```

####   _pages 폴더  및 필요한 .md파일 생성
이제 카테고리, 테그, about 등 필요한 페이지 정보들을 삽입합니다. 요약해서 말하면 permalink에 쓰여진 url로 요청이 들어오면 layout에 지정된 즉 _layout에 존재하는 {파일명}.html을 불러와 삽입합니다.
[https://github.com/7271kim/7271kim.github.com/tree/master/_pages]({{"https://github.com/7271kim/7271kim.github.com/tree/master/_pages"}}){:target="_blank"} <br>
<br>
**category-archive.md에 대한 예시**

```java
---
title: "Posts by Category"
layout: categories
permalink: /categories/
author_profile: true
---

```

####  _config.yml 수정
하단 defaults: 부분에 _pages 부분을 추가합니다. 
[https://github.com/qmakzl/qmakzl.github.com/blob/master/_config.yml]({{"https://github.com/qmakzl/qmakzl.github.com/blob/master/_config.yml"}}){:target="_blank"} 

```java
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: true
      comments: true
      share: true
      related: true
  
  # _pages
  - scope:
      path: "_pages"
      type: pages
    values:
      layout: single
      author_profile: true
```

### js 빌드를 위한 설정
minimal-mistakes 테마의 경우 node.js를 이용하여 js를 쉽게 minify하거나 원하는 js 플러그인들을 하나의 파일로 합칠 수 있습니다. Node.js를 이용하고 싶지 않을 경우 assets/js/main.min.js에 원하는 스크립트 부분을 추가하면 됩니다. 필자는 Node.js까지 설치하고 /assets/js/custom/custom.js로 추가적인 js 파일을 만들고 템플릿에서 사용할 js를 여기에 집어 넣겠습니다.

#### Node.js 설치
[https://nodejs.org/en/]({{"https://nodejs.org/en/"}}){:target="_blank"} 해당 사이트에서 Node.js 12.14.0 버전을 다운로드 후 인스톨합니다. 

#### package.json 수정
개발환경은 minify 옵션을 제거하고 파일만 합친 형태로 진행하고 배포시에 minify옵션을 설정하도록 세팅 진행하겠습니다. <br>
[https://github.com/qmakzl/qmakzl.github.com/blob/master/package.json]({{"https://github.com/qmakzl/qmakzl.github.com/blob/master/package.json"}}){:target="_blank"} <br>

① devDependencies안에 해당 내용을 추가합니다.
```java
concat": "^1.0.3"
```

② scripts에 하위 내용 추가 
```
"watchDev:js": "onchange \"assets/js/**/*.js\" -e \"assets/js/main.min.js\" -- npm run buildDev:js",
"concat-js": "concat -o assets/js/main.min.js (요약)... assets/js/custom/custom.js(추가부분)",
"buildDev:js": "npm run concat-js && npm run add-banner"
"uglify": "uglifyjs (요약)... assets/js/custom/custom.js -c -m -o assets/js/main.min.js(추가 부분)",
```

#### cmd창에서 명령어 실행
package.json이 존재하는 위치에서 해당 명령어를 실행 하면 현재 /assets/js/ 아래에 있는 js파일들이 main.min.js에 합쳐서 나옵니다. uglifyjs 옵션에 대해 더 알고자 한다면 아래 문서를 참고하시면 됩니다.
[http://fibjs.org/ko/docs/awesome/module/uglify-js.md.html]({{"http://fibjs.org/ko/docs/awesome/module/uglify-js.md.html"}}){:target="_blank"}

```
# npm install
```
**개발시**
```
# npm run watchDev:js
```
**배포시**
```
# npm run build:js
```


이상으로 블로그를 처음 시작하시는 분들에게 도움이 되었길 바라며 GitBlog 시작하기에 대한 포스팅을 마치겠습니다. 감사합니다.
{: .notice--info}
