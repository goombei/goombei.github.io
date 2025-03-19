---
title: Github 블로그 시작하기
description: >-
  Jekyll 을 활용한 Github 블로그 만들기
author: goombei
date: 2025-03-19 15:46:00 +0900
categories: [Blog, github]
tags: 
  - blog
  - github
  - jekyll
  - rocky
  - ruby
  - wsl
pin: true
media_subpath: '/posts/20250319'
---

# 사용 SW 버전
WSL 2.4.12.0  
Rocky Linux 9.2  
Jekyll 4.4.1  
Chirpy 7.2  
Ruby 3.0.7 -> ruby 3.1.0  


# 테마 선정
[Chirpy][chirpy]  



# ruby 설치

https://jekyllrb.com/docs/installation/  
https://jekyllrb.com/docs/installation/other-linux/  

```bash
sudo dnf install ruby ruby-devel
sudo dnf group install "Development Tools"
```

이후 작업 ubuntu와 동일  
https://jekyllrb.com/docs/installation/ubuntu/

```bash
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

gem install jekyll bundler
```


```bash
# localhost 동작 test
jekyll new myblog
cd myblog
bundle exec jekyll serve

# bundle exec jekyll serve --livereload # 수정시 자동 적용
```

결과 OK  



# 테마 적용 

## 내 repo:
https://github.com/goombei/goombei.github.io

```bash
# cd <원하는 경로>
cd ~/git/goombei.github.io

git clone https://github.com/goombei/goombei.github.io
```

```bash
unzip jekyll-theme-chirpy-master.zip -d goombei.github.io
```

# push test

```bash
cd <username>.github.io
cd goombei.github.io
echo "Hello World" > index.html
git add -A
git commit -m "Start git blog"
git push -u origin main
```

# 다운로드 및 압축 해제



```bash
# 실행
bundle install
```

## 오류사례 

```bash
Could not find compatible versions

Because every version of jekyll-theme-chirpy depends on Ruby ~> 3.1
  and Gemfile depends on jekyll-theme-chirpy >= 0,
  Ruby ~> 3.1 is required.
So, because current Ruby version is = 3.0.7,
  version solving has failed.
```

아래 명령 실행해서 ruby 모듈 설치  

```bash
sudo dnf module install ruby:3.1
```

https://docs.redhat.com/ko/documentation/red_hat_enterprise_linux/9/html/9.1_release_notes/enhancement_dynamic-programming-languages-web-and-database-servers#enhancement_dynamic-programming-languages-web-and-database-servers  


```bash
# 구동
jekyll serve
```

## 오류 발생

```bash
# 해결 방법 1
sudo gem pristine --all
```

안됨  
결과 하나씩 확인한 결과 문제 gem 발견  

```bash
# sudo gem pristine sass-embedded --version 1.69.5
```


아래 방법으로 해결  

```bash
sudo gem update --system

bundle install
```

https://github.com/jekyll/jekyll/issues/9563#issuecomment-1998633620  


## git push

```bash
git add -A

git commit -m "local ok"

git push
```

## 오류
페이지를 들어가보면 아래 내용만 보이고 제대로 나오지 않음  

```
--- layout: home # Index page ---
```

아래 사이트를 참고했으나 해결되지 않음  
https://delaying.github.io/posts/layouthome/  


## 해결
참고 페이지:  
https://github.com/cotes2020/jekyll-theme-chirpy/discussions/1809#discussioncomment-10588562  

내 repo에서 아래 메뉴 접근  
Settings - Pages - Build and deployment - Source: Github Actions  

그러면 자동으로 아래 파일을 생성. 바로 편집할 수 있는 버튼이 나타난다.  

```
.github/workflows/jekyll.yml
```

중간에 아래 내용을 삽입  

```yaml
…
- name: Setup Pages
…
# 삽입할 내용
- name: npm build
  run: npm install && npm run build
# 삽입할 내용 끝
- name: Build with Jekyll
…
```

최종 commit  

Actions 탭 확인 결과 **완료!!**  


# 추가 조치
## 게시물에서 author가 보이지 않는 경우

```yaml
vi _data/authors.yml

# 들어갈 내용 시작
goombei:
  name: goombei
  twitter:
  url: https://github.com/goombei/
# 들어갈 내용 끝
```


게시물에서 아무데나 아래 줄 추가  

```markdown
{%raw%}{% assign author = site.data.authors[page.author] %}{%endraw%}
```


[chirpy]: https://www.irgroup.org/posts/jekyll-chirpy/
{% assign author = site.data.authors[page.author] %}
