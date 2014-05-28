# 어플리케이션 레이아웃 작업

이전 섹션에서 웹페이지에 표시되었던 컨텐츠들이 이쁘게 보이지 않았다. 이제는 어플리케이션 레이아웃을 아래와 같이 변경하자.
그리고 웹페이지를 다시 보면, 아래와 같이 보일 것이다.

```html
<body>
    <!-- Fixed navbar -->
    <div class="navbar navbar-default navbar-fixed-top" role="navigation">
      <div class="container">
        <div class="navbar-header">
          <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
            <span class="sr-only">Toggle navigation</span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
          </button>
          <a class="navbar-brand" href="#">AuthBlog</a>
        </div>
        <div class="navbar-collapse collapse">
          <ul class="nav navbar-nav">
            <li class="active"><a href="#">Home</a></li>
            <li><a href="#about">About</a></li>
            <li><a href="#contact">Contact</a></li>
            <li class="dropdown">
              <a href="#" class="dropdown-toggle" data-toggle="dropdown">Dropdown <b class="caret"></b></a>
              <ul class="dropdown-menu">
                <li><a href="#">Action</a></li>
                <li><a href="#">Another action</a></li>
                <li><a href="#">Something else here</a></li>
                <li class="divider"></li>
                <li class="dropdown-header">Nav header</li>
                <li><a href="#">Separated link</a></li>
                <li><a href="#">One more separated link</a></li>
              </ul>
            </li>
          </ul>
        </div><!--/.nav-collapse -->
      </div>
    </div>

    <div class="container">

      <%= yield %>

    </div> <!-- /container -->
</body>
```

이제, 브라우저 화면을 다시 로드하면 아래와 같이 보이게 된다.

![](http://i1373.photobucket.com/albums/ag392/rorlab/Photobucket%20Desktop%20-%20RORLAB/auth_blog/2014-05-28_14-16-40_zpsa79d2599.png)

그러나 자세히 보면 상단 메뉴바가 `<body>` 태그로 나타내는 부분이 가려지게 된다. 이 때는 `app/assets/stylesheets/application.css.scss` 파일을 열어 `body` 태그에 `margin-top:60px;`을 추가해 주면 깔끔하게 보이게 된다.

![](http://i1373.photobucket.com/albums/ag392/rorlab/Photobucket%20Desktop%20-%20RORLAB/auth_blog/2014-05-28_15-37-03_zps2001566c.png)

## flash 메시지 표시하기

지금까지는 `flash` 메시지가 제대로 보이지 않았다. 세가지 작업을 추가하면 아래와 같이 이쁘게 보이게 할 수 있다.

![](http://i1373.photobucket.com/albums/ag392/rorlab/Photobucket%20Desktop%20-%20RORLAB/auth_blog/2014-05-28_15-42-16_zps52949be1.png)

`app/helpers/application_helper.rb` >

```ruby
module ApplicationHelper
  def bootstrap_class_for(flash_type)
    case flash_type
      when "success"
        "alert-success"   # Green
      when "error"
        "alert-danger"    # Red
      when "alert"
        "alert-warning"   # Yellow
      when "notice"
        "alert-info"      # Blue
      else
        flash_type.to_s
    end
  end
end
```

`app/views/layouts/application.html.erb` >

```html
<div class="container">
  <%= render partial: "shared/flash_messages", flash: flash %>
  <%= yield %>
</div>
```

`app/views/shared/_flash_messages.html.erb` >

```html
<% flash.each do |type, message| %>
  <div class="alert <%= bootstrap_class_for(type) %> alert-dismissable fade in">
    <button type="button" class="close" data-dismiss="alert" aria-hidden="true">&times;</button>
    <%= message %>
  </div>
<% end %>
```

이제 `로그아웃` 링크를 클릭하면와 같이 `flash` 메시지가 보이게 된다.

![](http://i1373.photobucket.com/albums/ag392/rorlab/Photobucket%20Desktop%20-%20RORLAB/auth_blog/2014-05-28_16-06-20_zpsa1b4765d.png)

