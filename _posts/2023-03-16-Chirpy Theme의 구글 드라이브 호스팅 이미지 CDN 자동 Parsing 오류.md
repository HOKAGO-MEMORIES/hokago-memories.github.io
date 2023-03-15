---
layout : post
title : Chirpy Theme의 구글 드라이브 호스팅 이미지 CDN 자동 Parsing 오류
date : 2023-03-16
categories : [development, blog]
tags : [jekyll, chirpy, github pages, google drive, 클라우드, 이미지, 미리보기 오류]
---

## **jsdelivr를 통한 CDN 사용**
---  

깃허브 블로그를 이용하고 있다면 jedelivr를 통해 CDN을 사용할 수 있다. `CDN`은 전세계 각지에 많은 캐시 서버를 두어서, 사용자에 가장 가까운 서버에 대용량 파일을 저장하여, 빠른 로드를 할 수 있도록 한다.  
이 CDN 사용법은 정말 간단하다. 따로 설치하거나 그런 것 없이 하나의 링크만 만들어 주면 된다.  

>  https://cdn.jsdelivr.net/gh/ `(깃허브 유저 이름)/(리파지토리 이름)/@(브랜치 이름)/(파일 경로)`

블로그 리파지토리에 있는 `assets/img/minchotejava.jpeg` 파일을 이용하여 링크를 만들면 아래와 같다.  

> https://cdn.jsdelivr.net/gh/ `HOKAGO-MEMORIES/hokago-memories.github.io@main/assets/img/minchotejava.jpeg`

아니면 [Migrate from GitHub to jsDelivr](https://www.jsdelivr.com/github) 이곳을 통해 파일 경로를 붙여 넣어서 링크를 얻을 수도 있다.

<br>

## **Chirpy Theme에서의 자동 Parsing**
---

직접 위 링크를 만들기는 생각보다 귀찮은 작업이다. 그래서인지 이 블로그를 만들기 위해 이용한 Chirpy Theme 에서는 `_config.yml`의 `img_cdn` 항목에 위의 주소에서 브랜치 이름까지의 링크를 넣어주기만 하면 된다.  

``` yaml
# The CDN endpoint for images.
# Notice that once it is assigned, the CDN url
# will be added to all image (site avatar & posts' images) paths starting with '/'
#
# e.g. 'https://cdn.com'
img_cdn: https://cdn.jsdelivr.net/gh/HOKAGO-MEMORIES/hokago-memories.github.io@main
```
{:file="_config.yml"}

리파지토리에 있는 이미지 파일의 경로를 그대로 적어놓으면 알아서 바꿔준다.

```
![민트초코데자와](/assets/img/minchotejava.jpeg)
```

![민트초코데자와](/assets/img/minchotejava.jpeg)  

![자동생성이미지](https://drive.google.com/uc?export=view&id=1-wvdZQgLZ71VSL0nVrUXiUq72T91LlJM)

자동으로 링크가 바뀐 것을 볼 수 있다.

<br>

## **구글 드라이브 호스팅 이미지 Parsing 오류와 해결방법**
---

### **구글 드라이브 호스팅 이미지 Parsing 오류**  

[이전 포스트](https://hokago-memories.github.io/posts/%EA%B5%AC%EA%B8%80-%EB%93%9C%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%B4-jekyll-chirpy%EC%97%90-%EC%9D%B4%EB%AF%B8%EC%A7%80-%ED%98%B8%EC%8A%A4%ED%8C%85/) 를 살펴보면 이미지 업로드를 위해 구글 드라이브를 이용한다고 했는데, 여기서 문제가 발생했다.  

![오류](https://drive.google.com/uc?export=view&id=1-KXwm2U8jrveCu-Ag4CFiZHLIlc3H5nx)

![오류2](https://drive.google.com/uc?export=view&id=1-cU05XuxC5H90J4yOw77YE8tTaswM7xa)

리파지토리에 있는 이미지가 아닌 구글 드라이브에 있는 이미지를 CDN 링크로 만들어 버렸다.  
밑의 src에 있는 `https://drive.google.com/~`의 링크를 그대로 사용하면 되는데 이를 `https://cdn.jsdlivr.net/gh/~`로 만들어버린 것이다!  
저 경로에는 아무런 파일이 없기 때문에 이미지를 클릭하면 당연히 아무런 이미지가 뜨질 않는다.  

### **자동 Parsing 코드 확인**

여기저기 알아보는 중에 `_includes`폴더에 있는 `refactor-content.html`에서 자동 Parsing 기능을 한다는 것을 알아냈는데, 처음 보는 언어가 나를 맞이해줬다.   
`Liquid`라고 하는 언어인데, 기본적인 설명은 [Liquid template language (shopify.github.io)](https://shopify.github.io/liquid/) 이곳에 있다.  


{% raw %}
``` liquid
<!-- images -->

{% assign IMG_TAG = '<img ' %}

{% if _content contains IMG_TAG %}
  {% assign _img_content = nil %}
  {% assign _img_snippets = _content | split: IMG_TAG %}

  <!-- CDN URL -->
  {% if site.img_cdn %}
    {% if site.img_cdn contains '//' %}
      {% assign _path_prefix = site.img_cdn %}
    {% else %}
      {% assign _path_prefix = site.img_cdn | relative_url %}
    {% endif %}
  {% else %}
    {% assign _path_prefix = site.baseurl %}
  {% endif %}

  <!-- Add image path -->
  {% if page.img_path %}
    {% assign _path = page.img_path | append: '/' | replace: '//', '/' %}
    {% assign _path_prefix = _path_prefix | append: _path %}
  {% endif %}

  {% for _img_snippet in _img_snippets %}
    {% if forloop.first %}
      {% assign _img_content = _img_snippet %}
      {% continue %}
    {% endif %}
  
    {% assign _left = _img_snippet | split: '>' | first %}
    {% assign _right = _img_snippet | remove: _left %}

    {% unless _left contains 'src=' %}
      {% continue %}
    {% endunless %}
  
    {% assign _left = _left | remove: ' /' | replace: ' w=', ' width=' | replace: ' h=', ' height=' %}
    {% assign _attrs = _left | split: '" ' %}

    {% assign _width = nil %}
    {% assign _height = nil %}
    {% assign _lqip = nil %}
    {% assign _class = nil %}

    {% for _attr in _attrs %}
      {% unless _attr contains '=' %}
        {% continue %}
      {% endunless %}

      {% assign _pair = _attr | remove: '"' | split: '=' %}
      {% capture _key %}{{ _pair | first }}{% endcapture %}
      {% capture _value %}{{ _pair | last }}{% endcapture %}

      {% case _key %}
        {% when 'width' %}
          {% assign _width = _value %}
        {% when 'height' %}
          {% assign _height = _value %}
        {% when 'src' %}
          {% assign _src = _value %}
        {% when 'lqip' %}
          {% assign _lqip = _value %}
        {% when 'class' %}
          {% assign _class = _value %}
      {% endcase %}

    {% endfor %}
```
{:file="refactor-content.html"}
{% endraw %}

파일의 일부분을 잘라왔는데 만약 이미지 링크에 `//`가 없다면,  우리가 위에서 적어놓은 `img_cdn`에 있는 내용을 링크에 추가하는 것이다.  
그런데 이상하다. 구글 드라이브에 호스팅한 이미지들은 주소가 `https://drive.google.com/~`로 시작해서 `//`을 포함하는데 cdn 링크로 변질되었다!  

그 이유는 52번째 줄에서부터 있었다.

<br>

{% raw %}
``` liquid
{% assign _pair = _attr | remove: '"' | split: '=' %}
{% capture _key %}{{ _pair | first }}{% endcapture %}
{% capture _value %}{{ _pair | last }}{% endcapture %}
```
{:file="refactor-content.html"}
{% endraw %}

이게 그 문제의 코드다. `<img> 태그`를 받아서 `"`문자는 지우고, 문자를 `=`로 나눠 맨 앞에 있는 값은 `_key` 값으로, 맨 뒤에 있는 값은 `_value` 값으로 하여 `[key, value]`의 리스트로 만들겠다는 것이다..  

>위에서 봤던 `/assets/img/minchotejava.jpeg`를 예로 들어보자.  
>이를 img 태그로 표현하면 `<img src="/assets/img/minchotejava.jpeg">`가 되는데,  
>`_pair`는 `[src, /assets/img/minchotejava.jpeg]`가 되는 것이다.

이대로 이용하면 아무런 문제가 없다. `=`문자가 src와 이미지 링크를 나누는 데 하나만 사용되었기 때문이다.  
하지만 구글 드라이브에 호스팅해서 얻은 주소로 예를 들어보자.

> 이미지 주소는  `https://drive.google.com/uc?export=view&id=1-6S4m0AHyLF6xtO6zkzdTxQy2Lg5diys` 와 같다.  
> 이를 `=`문자로 나누게 되면 `[src, https://drive.google.com/uc?export, view&id, 1-6S4m0AHyLF6xtO6zkzdTxQy2Lg5diys]` 가 되어 버린다.  
> `_pair`는 `[src, 1-6S4m0AHyLF6xtO6zkzdTxQy2Lg5diys`]`가 되는 셈이다.

따라서 주소가 아닌 문자를 값으로 받았기 때문에 img_cdn에 뒤의 값을 집어넣어버린 것이다.

![예시](https://drive.google.com/uc?export=view&id=10BZPGdrrACNvK2wG1macnT_08hZlEYqa)

위 이미지를 보면 img_cdn 뒤에 `_value` 값이 붙은 것을 볼 수 있다.

### **문제 해결**

구글 드라이브에서 호스팅한 이미지들은 무조건 `=`문자가 3개 이상 들어간다는 점을 이용했다.  

{% raw %}
``` liquid
<!--구글드라이브 파싱 오류 해결-->
{% if _pair.size > 2 %}
  {% capture _key %}{{ _pair | first }}{% endcapture %}
  {% capture _value %}{{ _pair | slice: 1, 3 | join: '=' }}{% endcapture %}
{% else %}
  {% capture _key %}{{ _pair | first }}{% endcapture %}
  {% capture _value %}{{ _pair | last }}{% endcapture %}
{% endif %}
```
{:file="refactor-content.html"}
{% endraw %}

`_pair`의 사이즈가 2이하라면 기존에 했던 방식을 이용하고, 2를 넘는다면 `_pair`의 두번째 값에서부터 총 3개의 값을 잘라내어 합치도록 했다. 근데 그냥 이대로 합쳐버리게 된다면 `=`문자는 없어져서 기존의 주소가 나오지 않기 때문에 합칠 때 `=`문자를 포함하도록 했다.  
이 문법은 [Liquid filters: slice (shopify.dev)](https://shopify.dev/docs/api/liquid/filters/slice) 이곳에서 참고했다.   

이 방법을 이용하면 `_pair`는 `[src, https://drive.google.com/uc?export=view&id=1-6S4m0AHyLF6xtO6zkzdTxQy2Lg5diys]`로 우리가 원하는 주소가 그대로 들어가게 된다!

![정상이미지](https://drive.google.com/uc?export=view&id=1-XndPvtlWDmdJSD4UZR8zLJZ5qB9WuO-)

![정상이미지2](https://drive.google.com/uc?export=view&id=1-WSURu6WxzIsXKkfFo9D6AxoCdHbavL_)

이제 자기 혼자 CDN 링크로 바뀌지 않고 구글 드라이브로 호스팅해서 얻은 주소 그대로 사용한 것을 볼 수 있다!  
내가 정보를 잘 못찾는 것인지 이 문제에 관련된 정보가 얼마 없어서 문제해결하는데 며칠이 걸렸었는데, 이렇게 해결해서 다행이다.  