# 사용자 인증 적용하기

`devise`는 모든 컨트롤러와 액션에서 사용할 수 있는 헬퍼메소드를 제공해 준다.

* `before_action :authenticate_user!`
* `current_user`
* `user_signed_in?`
* `user_session`

## Post 리소스의 생성

인증 효과를 검증하기 위해서 여기서 `Post` 리소스를 생성하기로 하자.

```bash
$ rails g scaffold Post title content:text user:references
      invoke  active_record
      create    db/migrate/20140528081107_create_posts.rb
      create    app/models/post.rb
      invoke    test_unit
      create      test/models/post_test.rb
      create      test/fixtures/posts.yml
      invoke  resource_route
       route    resources :posts
      invoke  scaffold_controller
      create    app/controllers/posts_controller.rb
      invoke    erb
      create      app/views/posts
      create      app/views/posts/index.html.erb
      create      app/views/posts/edit.html.erb
      create      app/views/posts/show.html.erb
      create      app/views/posts/new.html.erb
      create      app/views/posts/_form.html.erb
      invoke    test_unit
      create      test/controllers/posts_controller_test.rb
      invoke    helper
      create      app/helpers/posts_helper.rb
      invoke      test_unit
      create        test/helpers/posts_helper_test.rb
      invoke    jbuilder
      create      app/views/posts/index.json.jbuilder
      create      app/views/posts/show.json.jbuilder
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/posts.js.coffee
      invoke    scss
      create      app/assets/stylesheets/posts.css.scss
      invoke  scss
      create    app/assets/stylesheets/scaffolds.css.scss
```

`Post` 모델의 속성으로 지정한 `user:references`는 두가지 일을 한다. 하나는 foreign key로 `user_id:integer`를 만들고 인덱스도 동시에 생성한다.

```ruby
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :title
      t.text :content
      t.references :user, index: true

      t.timestamps
    end
  end
end
```

다른 하나는 `Post` 모델 클래스에 `belongs_to :user`를 자동으로 추가해 준다.

```ruby
class Post < ActiveRecord::Base
  belongs_to :user
end
```

물론 `user_id:integer:index`와 같이 지정해도 되지만, 이 때는 `Post` 모델 클래스에 `belongs_to :user`를 추가해 주지 않아 직접 작성해 주어야 한다.

```ruby
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :title
      t.text :content
      t.integer :user_id

      t.timestamps
    end
    add_index :posts, :user_id
  end
end
```

그리고 데이터베이스 마이그레이션을 한다.

```bash
$ rake db:migrate
== 20140528081107 CreatePosts: migrating ======================================
-- create_table(:posts)
   -> 0.0048s
== 20140528081107 CreatePosts: migrated (0.0049s) =============================
```

`User` 모델 클래스(app/models/user.rb)에는 아래와 같이 관계 선언을 추가해 주어 `Post` 모델(app/models/post.rb)에 대해서 일대다로 연결되도록 한다.

```ruby
class User < ActiveRecord::Base
  has_many :posts, dependent: :destroy
end
```

`has_many` 메소드에 사용한 `dependent: :destroy` 옵션은 특정 `user` 객체를 삭제할 때 연관된 모든 `post` 객체들을 한꺼번에 삭제하도록 해 주어야 하는데, 이는 `user_id` 값이 없는 `post` 레코드들(`'orphan', 부모 없는 고아` )은 의미 없는 쓰레기 데이터가 될 수 있기 때문이다.

## Post 리소스에 대한 접근제한하기




