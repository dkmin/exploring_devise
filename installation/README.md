# Devise 젬의 설치 및 셋업


## 시스템 구성

이 책에서는 보다 효과적인 설명을 위해서 실제로 작은 프로젝트를 작성하면서 설명을 할 것이다. 따라서 샘플 프로젝트를 작성하는 개발환경은 아래와 같다.

* Ruby version : 2.1.2p95 (2014-05-08 revision 45877)
* Rails version : 4.1.1

## 샘플 프로젝트의 생성

`Devise` 젬의 이해를 돕기 위해 간단한 샘플 프로젝트를 만들기 위해서 아래와 같이 커맨트 쉘에서 명령을 실행한다.

```bash
$ rails new devised_blog
```

여기서는 `Devise` 젬의 사용법에 집중하기 위해서 레일스의 디폴트 데이터베이스인 `sqlite3`을 사용하도록 하겠습니다.


## 젬 설치 및 셋업

레일스 프로젝트의 루트에 위치한 `Gemfile`을 열어 아래와 같이 추가한 후,

```ruby
gem 'devise'

# 페이지 뷰를 깔끔하게 보이도록 하기 위해서 아래의 두 젬을 추가로 설치한다.
gem 'bootstrap-sass'
gem 'simple_form'
```

설치한다.

> **Warning** 레일스의 최근 버전에서 새로운 프로젝트를 생성하면 `sass-rails` 젬이 디폴트로 추가되어 있지만, 혹시 누락되었다면 함께 추가해 주어야 `SASS` 형식으로 `CSS`를 사용할 수 있게 된다.

```bash
$ bundle install
```

> **Note** 이 책의 샘플 프로젝트는 레일스 4를 기준으로 작성할 것이지만 레일스 3.2.x 버전을 사용해야 할 경우는 [`여기`](https://github.com/twbs/bootstrap-sass#rails-32x)를 참고하기 바란다.

## 'bootstrap-sass' 젬의 셋업 및 사용법

[`bootstrap-sass`](https://github.com/twbs/bootstrap-sass) 젬은 `Twitter-Bootstrap`을 `SASS` 형식으로 사용하기 위한 것이다.


### Sass 셋업

`Bootstrap`에서 제공하는 모든 스타일, 믹신(mixin), 그리고 변수들을 사용할 수 있도록 하기 위해서는 `Sass` 파일인 `application.css.scss` 파일로 `Bootstrap`을 임포트해야만 한다. 여기서는 중요한 것은 `//= require` 지시어를 사용하지 말아야 한다. 왜냐하면 이 지시어를 사용하면 다른 CSS 파일들이 `Bootstrap`의 믹신이나 변수에 접근할 수 었게 되기 때문이다. 따라서 아래와 같이 `@import`를 사용하여 추가한다.

```css
@import "bootstrap";
```

옵션이기는 하지만, 추가로 아래와 같이 `Bootstrap` 테마를 포함할 수도 있다.

```css
@import "bootstrap/theme";
```

`Boostrap`에서 정의한 변수들의 목록은 [`여기`]()를 참고할 수 있는데, 아래와 같이 `@import` 지시어 전(위치가 중요함)에 특정 변수들을 재정의하면 간단하게 변수들을 오버라이드할 수 있다.

```css
$navbar-default-bg: #312312;
$light-orange: #ff8c00;
$navbar-default-color: $light-orange;

@import "bootstrap";
```

때로는 명시적으로 `Bootstrap`의 각 요소들을 지정할 경우도 있다. 이 때는 아래와 같이 커맨드라인 쉘 명령을 실행하면 모든 요소를 포함하는 `bootstrap-custom.scss` 파일이 생성되는데, 이 파일을 열어 불필요한 요소는 코멘트 처리할 수도 있다.

```bash
cp $(bundle show bootstrap-sass)/vendor/assets/stylesheets/bootstrap.scss \
 app/assets/stylesheets/bootstrap-custom.scss
```

이 때는 아래와 같이 `application.css.scss` 파일에 선언해 놓은 `@import 'bootstrap'`을 `@import 'bootstrap-custom'`으로 변경해 주어야 한다. 생성된 `bootstrap-custom.scss` 파일의 내용은 아래와 같다.

```css
// Core variables and mixins
@import "bootstrap/variables";
@import "bootstrap/mixins";

// Reset and dependencies
@import "bootstrap/normalize";
@import "bootstrap/print";
@import "bootstrap/glyphicons";

// Core CSS
@import "bootstrap/scaffolding";
@import "bootstrap/type";
@import "bootstrap/code";
@import "bootstrap/grid";
@import "bootstrap/tables";
@import "bootstrap/forms";
@import "bootstrap/buttons";

// Components
@import "bootstrap/component-animations";
@import "bootstrap/dropdowns";
@import "bootstrap/button-groups";
@import "bootstrap/input-groups";
@import "bootstrap/navs";
@import "bootstrap/navbar";
@import "bootstrap/breadcrumbs";
@import "bootstrap/pagination";
@import "bootstrap/pager";
@import "bootstrap/labels";
@import "bootstrap/badges";
@import "bootstrap/jumbotron";
@import "bootstrap/thumbnails";
@import "bootstrap/alerts";
@import "bootstrap/progress-bars";
@import "bootstrap/media";
@import "bootstrap/list-group";
@import "bootstrap/panels";
@import "bootstrap/responsive-embed";
@import "bootstrap/wells";
@import "bootstrap/close";

// Components w/ JavaScript
@import "bootstrap/modals";
@import "bootstrap/tooltip";
@import "bootstrap/popovers";
@import "bootstrap/carousel";

// Utility classes
@import "bootstrap/utilities";
@import "bootstrap/responsive-utilities";
```


> **Info** 전통적인 CSS에서는 중첩태그나 믹신 또는 변수를 사용할 수 없으나 이와 같은 것을 보강한 CSS의 확장된 버전을 `Sass`라고 한다. `Sass`는 두가지 형태의 파일포맷을 가지는데, `.sass` 확장자를 가지는 파일은 위의 기능에다가 `{ }`을 스페이스 들여쓰기로 대신한 원조 `Sass` 형태이고, 이에 반하여 `.scss` 확장자를 가지는 파일(`Sassy CSS`)은 이러한 추가기능이 없는 변형된 `Sass` 형태이다. 일반적으로 레일스에서는 `.css.scss` 확장자를 가지는 `Sassy CSS` 파일형태를 주로 사용한다.

### 자바스크립트 셋업







