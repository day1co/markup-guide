# 패스트캠퍼스 마크업 가이드

패스트캠퍼스 사내 표준 가이드를 목적으로 작성되었습니다.

간결하고 코드의 논리적 표현을 목적으로 합니다.

패스트캠퍼스 엔지니어들과 논의하여 업데이트할 수 있습니다.

------

TOC
---
- [Global Rules](#global-rules)
- [HTML](#html)
  * [Doctype](#doctype)
  * [Meta Tags](#meta-tags)
    + [Encoding](#encoding)
    + [IE Compatible](#ie-compatible)
    + [Viewport](#viewport)
    + [Not used](#not-used)
  * [Self-closing](#self-closing)
  * [Attributes](#attributes)
  * [Boolean attributes](#boolean-attributes)
  * [Outline & Semantics](#outline--semantics)
    + [Outline](#outline)
    + [Semantics](#semantics)
  * [Responsive Image](#responsive-image)
    + [Image srcset](#image-srcset)
    + [Resolution Switching](#resolution-switching)
    + [Art Direction](#art-direction)
  * [Entity References](#entity-references)
- [Vue](#vue)
  * [Attributes Declaration](#attributes-declaration)
  * [Directive shorthands](#directive-shorthands)
- [CSS (SCSS)](#css--scss-)
  * [Architecture](#architecture)
  * [Basic](#basic)
  * [Units](#units)
  * [Formatting](#formatting)
  * [Declaration](#declaration)
  * [Nesting & Selector](#nesting--selector)
  * [Mediaquery](#mediaquery)
  * [Mixin](#mixin)
  * [Extend](#extend)
- [SEO](#seo)
  * [Metadata](#metadata)
  * [Social Discovery](#social-discovery)
    + [OGP (Open Graph Protocol)](#ogp-open-graph-protocol)
    + [Twitter Card](#twitter-card)
  * [Headings](#headings)
  * [Links](#links)
  * [Images](#images)
- [Accessibility](#accessibility)
  * [Focus](#focus)
  * [Text alternatives](#text-alternatives)
  * [Skip Contents](#skip-contents)
  * [Hiding content](#hiding-content)
- [Code Fomatting](#code-fomatting)
  * [Editconfig](#editconfig)
  * [Code style](#code-style)
  * [Stylelint](#stylelint)
------

# Global Rules

- Character set UTF-8
- 2 Spaces Soft Tab

# HTML

## Doctype

HTML5 Doctype

```html
<!doctype html>
```

## Meta Tags

### Encoding

인코딩 선언 `utf-8` 권장

```html
<meta charset="utf-8" />
```

### IE Compatible

IE 브라우저 호환 설정, `IE=edge` 값으로 최신 버전으로 동작

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge" />
```

### Viewport

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

### Not used

```html
// bad
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<meta name="viewport" content=".., .., maximum-scale=1.0, user-scalable=0" />
```

`IE=edge,chrome=1` [*chrome frame plugin*](https://www.chromium.org/developers/how-tos/chrome-frame-getting-started) 2014년 지원 중단

`Content-Type` 폐기 `charset` 권장

`maximum-scale`, `user-scalable` 화면을 제어할 필요가 있다면 사용자가 스스로 하도록 제공

## Self-closing
단일 태그는 `>` 앞 공백과 `/` self-closing 을 작성하여 _명시적으로_ 닫음
```html
<br />
<input type="text" />
```
> [Void elements](https://www.w3.org/TR/html5/syntax.html#void-elements) 의 `/` 문자는 요소에 영향을 미치지 않음

(HTML 5.2 W3C Recommendation)

## Attribute

속성(attr)은 `""` 큰따옴표 적용 사용

속성 사용 순서는 태그의 기본 필수 속성, CSS Selector 우선순위가 높은 순서, 커스텀 속성 순으로 작성

1. `required attribute`
2. `id`, `class`
3. `alt`, `title`
4. `role`, `data-*`, `aria-*`

## Boolean attributes

불린 속성은 선언된 자체로 `true` 로 간주

```html
// bad
<input type="text" disabled="">
<input type="text" disabled="disabled">

// good
<input type="text" disabled>
```

## Outline & Semantics

### Outline

**용어 설명**

섹션 콘텐츠 : `<article>, <section>, <nav>, <aside>`

헤딩 콘텐츠 : `<h1> ~ <h6>`

---

문서 개요 : 섹션 콘텐츠와 헤딩 콘텐츠를 활용하여 문서 개요를 제목 구조로 구성

- `<h1>` 은 문서의 `<header>` 에서 **한번만** 선언
- `<header>` 를 제외한 컨텐츠는 `<h2>` 로 시작
- `<section>`,  `<article>` 의 자식 요소로 헤딩 콘텐츠를 **반드시** 포함

> 문서 개요 알고리즘의 구현으로 `<section>`, `<article>` 에서 `<h1>` 은 여러번 선언 할 수 있지만 브라우저 또는 보조 기술에서의 문서 개요 지원 여부가 확실하지 않음

([HTML 5.2 W3C Recommendation](https://www.w3.org/TR/html52/sections.html#creating-an-outline))

```html
<body>
 <header class="header"> 
  <h1 class="header__logo">
   <a href="/" class="lheader__logo__a"> 
    <img src="fastcampus-logo.svg" alt="fastcampus" /> 
   </a>
  </h1>
 </header>
	
 <main id="main" class="main" role="main">
  <section class="section">
   <h2 class="section__title"> Section 1 </h2>

   <article class="section__article">
    <h3 class="section__article__title"> Article </h3>
   </article>
  </section>

  <section class="section">
   <h2 class="section__title"> Section 2 </h2>
  </section>
 </main>
	
 <footer class="footer">
  <h2 class="footer__title">FAST CAMPUS<h2>
 </footer>
</body>
```

### Semantics

*Block-level* 요소는 *Inline-level* 요소 안에서 선언할 수 없음

태그가 가진 고유 의미를 고려하여 사용

- `<strong>` : 문맥의 가장 중요한 부분을 강조
- `<em>` : 문맥의 일부 강조
- `<mark>` : 관련성을 표현
- `<cite>` : 작품의 이름
- `<dfn>` : 용어의 정의
- `<i>` , `<b>` : 대체할 의미의 태그가 없을 때 사용 (e.g. 숫자, 아이콘)

```html
// strong
<p> <strong> Notice ! </strong> ... </p>

// em : love vs you
<p> I <em>love</em> you </p> 
<p> I love <em>you</em> </p>

// i
<i class="count">0</i>
<i class="icon icon--facebook"> <img src="icon-facebook.svg" alt="facebook" /> </i>
```

## Responsive Image

### Image srcset

`srcset` : 이미지 리소스와 공백 뒤에 이미지의 실제 `width` 를 `w` 단위로 지정

`sizes` : media query (미디어 조건문) 과 공백 뒤에 앞 조건이 참인 경우 이미지가 채울 슬롯의 너비
(슬롯은 가장 근접한 이미지가 채움)

```html
<img srcset="image-480w.jpg 480w, image-960w.jpg 960w"
     sizes="(max-width: 600px) 580px, 960px"
     src="image-960w.jpg" alt="">
```

### Resolution Switching

각 해상도에서의 대응해야 한다면 `sizes` 를 생략하고 다음과 같이 사용이 가능

```html
<img srcset="image-480w.jpg, image-960w.jpg 2x"
     src="image-960w.jpg" alt="">
```

### Art Direction

이미지의 오브젝트가 해상도에 따라 너무 크거나 작아질 때 상황에 따라 아트 워크를 적절한 이미지로 대응

```html
<picture>
 <source media="(max-width: 599px)" srcset="image-480w-portrait.jpg">
  <source media="(min-width: 600px)" srcset="image-600w-landscape.jpg">
  <img src="image-600w-landscape.jpg" alt="">
</picture>
```
    
## Entity References

HTML 구문 중  <, >, ", ', & 특수 문자는 HTML Character Entity 로 대체 사용

```html
< : &lt;
> : &gt;
" : &quot;
' : &apos;
& : &amp;
```
[▲ 목차로 돌아가기](#TOC)

------

# Vue

## Attributes Declaration

렌더링 관여 속성 - HTML 기본 속성 - 데이터 바인딩 - 이벤트 속성 순으로 작성

1. List Rendering : `v-for` 
2. Conditionals : `v-if` `v-show` `v-cloak`
3. Render Modifiers : `v-pre` `v-once`
4. Unique : `key` `ref` `slot`
5. HTML Attribute 
6. Two-Way Binding : `v-model` `v-bind` `:`
7. Misc 
8. Events : `v-on` `@`
9. Content : `v-html` `v-text`

## Directive shorthands

축약이 가능한 디렉티브( `:` `@` )는 축약하여 사용

[▲ 목차로 돌아가기](#TOC)

------

# CSS (SCSS)

## Architecture

[BEM](http://getbem.com/)(Block__Element—Modifier-state) 사용

- Block : 자체적으로 의미를 가진 독립 컴포넌트
- Element : *Block* 에 의미 적으로 연결된 블록의 자식 요소
- Modifier: *Block*, *Element* 의 플래그 (다른 상태, 변형)
- state: *Modifier* 플래그의 확장한 상태

```html
<div class="block">
  <span class="block__elem"> ... </span>

  <button class="button button--state-success">success</button>
  <button class="button button--state-danger">danger</button>
</div>
```

## Basic

*ID*, *Tag* 선택자 사용 지양

- *ID* :  CSS Selector 우선순위가 높아 스타일 Overriding 이 쉽지 않음
- *Tag* : 인터프리터의 CSS Selector 해석을 우측에서 좌측으로 하기 때문

`!important` : 대부분의 경우 CSS Selector 를 개선하여 제거 가능

```scss
// bad, DOM Node 중 input 태그를 찾고 그중 부모 #header 의 자식인 요소를 찾음
#header input {
  ...
}
```

## Units

브라우저 기본 폰트 사이즈를 초기화(`font-size: 62.5%`) 하여 `rem` 사용 : `1rem = 10px`

`%`, `em` : 부모의 폰트 사이즈에 영향을 받기 때문에 필요할 경우 유동적인 값으로 구성 가능

`vh`, `vw` : viewport 의존하는 값일 경우 제한적으로 사용 가능 

## Formatting

**용어 설명**

규칙 선언 : CSS Selector 와 `{}` 를 포함한 부분

속성 선언 : `{}` 내부의 property와 값

---

- `''` 홑 따옴표 사용
- `0` 은 단위를 사용하지 않고, 소수점 값에 `0` 은 작성하지 않음
- 규칙 선언의 여는 괄호 `{` 앞에 공백, 닫는 괄호는 `}` 다음 줄에 작성
- 규칙 선언의 다중 선택자는 줄 바꿈 작성
- 속성 선언의 축약 속성은 모든 값이 명시돼야 할 때 작성
- 속성 선언의 `:` 뒤 공백 작성,  선언의 끝 `;` 반드시 포함
- 속성 선언의 여러 속성 작성 시 `,` 뒤 공백 문자를 포함
- *Hex Color Code* 는 소문자로, 축약하여 사용

```scss
// bad
.btn,.btn--sm,.btn--lg{ // 규칙 선언
  margin:10px 0 0 0; // 속성 선언
  border-width:0px;
  padding: 0 10px 0 10px;
  background-color:rgba(0,0,0,0.5);
  background-image:url(url);
  color:#FFFF00;
}

// good
.btn,
.btn--sm,
.btn--lg {
  margin-top: 10px;
  border-width: 0;
  padding-left: 10px;
  padding-right: 10px;
  background-color: rgba(0, 0, 0, .5);
  background-image: url('url');
  color: #ff0;
}
```

## Declaration

요소의 레이아웃, 타이포그래피, 기타 순서로 작성하고 그 중 축약을 지원하는 속성은 축약 순서로 선언

```scss
.element {
  /* Position */
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 10;
  
  /* Box-model */
  box-sizing: border-box;
  display: flex;
  margin: 0;
  border: 1rem solid $white;
  padding: 0;
  width: 10rem;
  height: 10rem;
  
  /* Typhography */
  font: italic bold 2rem/1.5 'Noto Sans', sans-serif;
  
  font-style: italic;
  font-wieght: bold;
  font-size: 2rem;
  line-height: 1.5;
  font-family: 'Noto Sans', sans-serif;
  
  /* Misc */
  background: #f00 url('url') no-repeat center center cover;
  opacity: 1;
  transform: translate(50%, 50%);
  transition: all 2s ease-out;
}
```

## Nesting & Selector

CSS Selector, Nesting 중첩은 최대 세번으로 제한하고 중첩된 셀렉터 사이에는 공백 추가

```scss
// bad
// style.scss
.header {
  .input {
    .search {
      input {
        ...
      }
    }
  }
}

// style.css
.header .item .search input {}

// good
// style.scss
.header {
  
  &__item {
  }
  
  &__search {
  
    &__input {
      ...
    }
  }
}

// style.css
.header__item {}
.header__search {}
.header__search__input {}
```

## Mediaquery

Mobile First, Module 단위로 코드 작성

> 일반적으로 데스크 탑 디자인은 모바일 디자인에 비해 코드가 복잡하므로 최소 코드부터 작성

```scss
.nav {

  &__list {
  }
    
  @media (min-width: 60em) {
    &__list {
    }
  }

  @media (max-width: 59.99em) {
    &__list {
    }
  }
}
```

## Mixin

`@include` 는 속성 선언의 하단에 작성

```scss
.element {
  position: absolute;
  top: 50%;
  left: 50%;
  @include transform(translate(-50%,-50%);
}
```

## Extend

`@extend` 는 연관성을 형성시키므로 명백히 연관성이 있다면 사용할 수 있음. (기본적으로 **Mixin** 을 사용할 것을 권장)

```scss
// style.scss
%btn {
  box-sizing: border-box;
  display: inline-block;
  padding: 1rem 2rem;
}

.btn {
@extend %btn;

  &-success {
    @extend %btn;
    background-color: $success;
  }
  
  &-error {
    @extend %btn;
    background-color: $error;
  }
}

// style.css
.btn,
.btn-success,
.btn-error {
  box-sizing: border-box;
  display: inline-block;
  padding: 1rem 2rem;
}
.btn-success {
  ...
}
.btn-error {
  ...
}
```
[▲ 목차로 돌아가기](#TOC)

------
# SEO

## Metadata

`<title>`: 사이트 주제를 검색엔진에 제공

> 각 페이지마다 고유하고, 정확하고, 간단하지만 설명이 담긴 제목 사용

`<meta name="description">`: 페이지 내용을 요약하여 검색엔진에 제공

> 구글 검색엔진에서 스니펫으로 사용할 텍스트를 찾지 못하는 경우 description 으로 대체하여 사용 할 수 있기 때문에 각 페이지에 적절한 설명을 추가하는 것을 권장

```html
<head>
 <title>주제</title>
 <meta name="description" content="각 페이지의 내용을 요약해서 제공합니다"/>
</head>
```

## Social Discovery

### OGP (Open Graph Protocol)

페이스북 객체와 동일한 기능을 가질 수 있도록 메타데이터(property, content) 제공

기본 메타 데이터 *(필수)*

- `og:type` : 유형을 나타내는 문자열
- `og:title` : 제목
- `og:url` : 표준 URL
- `og:image` : 공유 게시물에 첨부된 이미지 URL

선택적 메타 데이터

- `og:description` : 설명
- `og:image:alt` : 공유 게시물에 첨부된 이미지의 대체 텍스트
- `og:locale` : 리소스 언어

OGP 가이드 : [ogp.me](http://ogp.me/)

OGP 디버깅 : [Facebook Sharing debugger](https://developers.facebook.com/tools/debug/sharing/) 

```html
<meta property="og:type" content="website">
<meta property="og:url" content="https://www.fastcampus.co.kr/">
<meta property="og:title" content="패스트캠퍼스 - 커리어 성장을 위한 최고의 실무교육 아카데미">
<meta property="og:description" content="Life-changing Education, 우리의 미션입니다">
<meta property="og:image" content="...">
```

### Twitter Card

Twitter 에서 사용할 수 있는 OGP 에 대한 확장 속성

`twitter` : 네임 스페이스가 지정된 메타 태그를 사용하여 메타데이터 설명

기본 메타 데이터 *(필수)*

- `twitter:card` : 공유 게시물의 카드 형태
- `twitter:title` : 간결한 제목

선택적 메타 데이터

- `twitter:site` : Twitter 사용자ID 제공
- `twitter:discription` : 공유 게시물을 간결하게 요약한 설명 (*제목을 다시 사용하거나 일반 서비스를 설명하는 것은 지양*)
- `twitter:image` : 공유 게시물의 내용을 나타내는 고유 이미지 (*서비스 로고, 반복되는 이미지의 일반적인 이미지는 사용 지양*)
- `twitter:image:alt` : 공유 게시물 이미지의 대체 텍스트

트위터 카드 가이드 :  [developer.twitter.com](https://developer.twitter.com/en/docs/tweets/optimize-with-cards/overview/abouts-cards)

트위터 카드 벨리데이터 : [Twitter Cards Validator](https://cards-dev.twitter.com/validator)

```html
<meta name="twitter:card" content="summary">
<meta name="twitter:title" content="패스트캠퍼스 - 커리어 성장을 위한 최고의 실무교육 아카데미">
```

## Headings

다음의 경우를 *Heading Tag* 사용에 유의

- 무질서하게 섞어 씀
- 타이틀 텍스트가 너무 김
- 한 페이지에서 과도하게 사용
- 페이지 구조를 정의하는 데 도움이 되지 않음
- 스타일을 지정할 때만 사용하고 구조를 나타내는 데 활용하지 않음
- `<em>` 및 `<strong>` 과 같은 다른 태그를 사용하는 것이 더 적절함

## Links

검색엔진은 링크 텍스트를 분석하여 연결되는 페이지의 정보를 얻기 때문에 링크 텍스트를 단순히 버튼, 보기, 이동 등으로 작성하는 것을 지양

## Images

이미지의 파일 이름과 대체 텍스트를 최적화하면 구글 이미지 검색 노출 가능

[▲ 목차로 돌아가기](#TOC)

------
# Accessibility

## Focus

포커스를 대체할 수단을 제공하지 않은 상태에서는 `outline` 스타일 제거 지양

```scss
// bad
&:focus {
  outline: none;
}
```

## Text alternatives

꾸밈 이미지에는 `alt=""` 빈값을 적용

`<figure>`, ` <figcaption>` 을 활용하여 적절한 대체 텍스트 제공

중복 콘텐츠 제공은 제공을 안하는 것 보다 못함

```html
// bad
<img src="visual-deco.png">

<figure>
  <img src="apple.png" alt="apple">
  <figcaption>apple</figcaption>
</figure>

// good
<img src="visual-deco.png" alt="">

<figure>
  <img src="apple.png" alt="">
  <figcaption>apple</figcaption>
</figure>
```

## Skip Contents

Skip Navigation : 사용자가 콘텐츠에 도달하기까지 반복되는 콘텐츠를 생략할 수 있는 수단 제공

```html
<a href="#main">Skip to main content</a>
```

`<p>` : 보조 장치에서 다음 문단으로 넘어갈 수 있도록 콘텐츠 구성

## Hiding content

콘텐츠 제공에 시각적으로 숨겨야 할 경우 아래의 이유로 [*Hiding content clip patten*](https://snook.ca/archives/html_and_css/hiding-content-for-accessibility) 을 사용 권장

- `display: none` 의 속성은 DOM Tree 에서도 삭제됨
- `width: 0; height: 0;` 크기가 0인 컨텐츠는 스크린 리더에서 읽히지 않음

```scss
// bad
.a11y {
  display: none;  
}

// good
.a11y {
  overflow: hidden;
  position: absolute !important;
  width: 1px;
  height: 1px;
  clip: rect(1px, 1px, 1px, 1px);
}
```
[▲ 목차로 돌아가기](#TOC)

------
# Code Fomatting

## Editconfig

프로젝트 root 에 `.editconfig`  파일 생성
```
# Editor configuration, see http://editorconfig.org
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
indent_size = 2
indent_style = space
max_line_length = 120
trim_trailing_whitespace = true
```

## Code style

포맷 자동화가 가능한 공용으로 사용 가능한 방법을 찾는 중..

prettier: (mac) shift + alt + command + P

refomat code: (mac) alt + command + L

## Stylelint

[Stylelint](https://www.npmjs.com/package/stylelint)...

[▲ 목차로 돌아가기](#TOC)

------
[Code Guide by @mdo](http://codeguide.co/) 기반으로 [레진 마크업 가이드](https://github.com/lezhin/markup-guide/blob/master/README.md)를 참고하였습니다.

[MDN](https://developer.mozilla.org), [W3C Recommendation](https://www.w3.org/TR/html52/), [웹 콘텐츠 접근성 가이드(WCAG)](https://www.w3.org/TR/WCAG20/), [Google Accessibility](https://developers.google.com/web/fundamentals/accessibility/), [Vue styleguide](https://kr.vuejs.org/v2/style-guide/index.html) 그리고 [A11Y Poject](https://a11yproject.com/about) 에 기반합니다.
