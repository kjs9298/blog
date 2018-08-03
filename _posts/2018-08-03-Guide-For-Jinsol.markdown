---
layout: post
title: Guide to make categories for Jinsol
date: 2018-08-03 18:58:00 +0900
description: It is the guide to make categories in this theme.
img: jekyll_logo.png
categories: [etc]
---

### 0. 카테고리 이름 정하기
* jeckyll에서는 게시글마다 `categories` 변수로 카테고리를 선언해준다.
* 본인이 지정할 카테고리 코드(?)를 정하자.
* 뒤이은 예시의 경우 backend, frontend, etc, machine-learning, english

### 1. 카테고리 만들기

![카테고리 부분](../assets/img/guide_0.png)

1) _includes 밑에 **categories.html** 생성
``` html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto" id="navbar-row">
            <li class="nav-item {% if page.url == '/' %} active {% endif %}">
                <a class="nav-link" href="{{ site.baseurl }}/">ALL</a>
            </li>
            <li class="nav-item dropdown
            {% if page.url == '/web/backend/' or page.url == '/web/frontend/' or page.url == '/web/etc/'%} active {% endif %}">
                <a class="nav-link dropdown-toggle cursor" onclick="clickDropdown('web-dropdown')"
                   role="button" data-toggle="dropdown"
                   aria-haspopup="true" aria-expanded="false">
                    Web
                </a>
                <div class="dropdown-menu" aria-labelledby="navbarDropdown" id="web-dropdown">
                    <a class="dropdown-item" href="{{ "/web/backend" | prepend: site.baseurl }}">Backend</a>
                    <a class="dropdown-item" href="{{ "/web/frontend" | prepend: site.baseurl }}">Frontend</a>
                    <div class="dropdown-divider"></div>
                    <a class="dropdown-item" href="{{ "/web/etc" | prepend: site.baseurl }}">ETC</a>
                </div>
            </li>
            <li class="nav-item {% if page.url == '/machine-learning/' %} active {% endif %}">
                <a class="nav-link" href="{{ "/machine-learning" | prepend: site.baseurl }}">Machine Learning</a>
            </li>
            <li class="nav-item {% if page.url == '/english/' %} active {% endif %}">
                <a class="nav-link" href="{{ "/english" | prepend: site.baseurl }}">English</a>
            </li>
            <li class="nav-item">
                <a class="nav-link disabled" href="#">비활성화 테스트</a>
            </li>
        </ul>
    </div>
</nav>
<script>
    function clickDropdown(dropdown_id) { // TODO : Refactoring, It have to be recycling to other elements.
        var x = document.getElementById(dropdown_id);
        if (x.style.display !== "block") {
            x.style.display = "block";
        } else {
            x.style.display = "none";
        }
    }
</script>

```

* li 마다 본인이 노출하고 싶은 카테고리명 적으심.
* 카테고리를 클릭하면 이동할 href를 수정하심.
* 드롭다운을 사용할 부분은 clickDropdown parameter 수정하셈

2) 메인에 카테고리 붙여주기
* 만들었으면 메인에 붙여줘야할 것 아님.
* /_layout/main.html 에서 content 위에다가 이렇게 붙여주셈

``` html
<div class="category-box">
  {% include categories.html %}
</div>
<div class="content-box clearfix">
  {{ content }}
</div>

```


### 2. 카테고리를 클릭 한 후 카테고리에 속한 글 목록 만들기

![카테고리별 글 목록](../assets/img/guide_2.png)
* _include 밑에 **list.html** 생성

``` html
{% for post in include.posts %}
<article class="post">
    {% if post.img %}
    <a class="post-thumbnail" style="background-image: url({{"/assets/img/" | prepend: site.baseurl | append : post.img}})" href="{{post.url | prepend: site.baseurl}}"></a>
    {% else %}
    {% endif %}
    <div class="post-content">
        <h2 class="post-title"><a href="{{post.url | prepend: site.baseurl}}">{{post.title}}</a></h2>
        <p>{{ post.content | strip_html | truncatewords: 15 }}</p>
        <span class="post-date">{{post.date | date: '%Y, %b %d'}}&nbsp;&nbsp;&nbsp;—&nbsp;</span>
        <span class="post-words">{% capture words %}{{ post.content | number_of_words }}{% endcapture %}{% unless words contains "-" %}{{ words | plus: 250 | divided_by: 250 | append: " minute read" }}{% endunless %}</span>
    </div>
</article>
{% endfor %}

```

* 카테고리마다 이 화면을 만들어줘야하면 귀찮잖슴.
* 그래서 posts 라는 변수에다가 우리가 뿌려줄 데이터를 집어 넣을 것임.
* 위에 include.posts 보이잖. 이 페이지를 사용하는 곳에서 posts에 노출시킬 글 목록을 집어넣을 것임.


### 3. 디렉토리마다 index.html 만들어주기

* 본인이 URL 규칙을 어떻게 하냐에 따라 다른데 나는 다음과 같이 함
* leaf 에만 글을 보관하려고 leaf 디렉토리에만 index.html을 각각 달아주었음
![index 디렉토리](../assets/img/guide_3.png)

* index.html

``` text
---
layout: main
---

{% include list.html posts=site.categories.machine-learning %}

```
* 아까 공통으로 정히해주었던 list.html을 포함만 시켜주면 됨
* post에다가는 site.categories.카테고리명 으로 post들을 불러와서 꼽아줌



### 끝!
* 끝났음 ~
