# Day 19. Template

##  Template

- 개발자도구 - source - css - boostratp에서 버전 확인하기

- gemfile에 추가하기

  ```
  # bootstrap 4
  gem 'bootstrap', '~> 4.1.1'
  # gem 'bootstrap-saas' 
  # turbolinks 주석처리
  ```

- *home.scss* 

  ![캡처](C:\Users\student\Desktop\캡처.PNG)

  ```scss
  @import 'icons.min';
  @import 'main';
  @import 'responsive';
  @import 'color-schemes/color';
  @import 'color-schemes/color1';
  @import 'color-schemes/color2';
  @import 'color-schemes/color4';
  @import 'color-schemes/color5';
  ```



- placehomder - css - 위에 대당하는 css 파일 찾아서 

  *vendor/assets/stylesheets*로 가져오기

- **

- css

  *home.coffee* -> *home.js*

  *assets.rb*

  ```
  Rails.application.config.assets.precompile += %w( home.js
                                                    home.scss)
  ```

- *vendor/asset/stylesheets/main.css*

  ```
  view port : 모바일 사이즈 일 때 뭐하는 거래
  ```



*css에서 오류가 발생한다면?*

- `$ rake assets:precompile`  

  - error가 난 곳을 확인할 수 있음

  - 모든 것에 'error' 표시가 사라질 때 까지 반복해서 실행하기

    - css 폴더에서 오류가 난 곳을 보니 

    ```
    body.dark .list-group-item.active:hover
    {
        border-color: #009efb;
    }
    ```

> Precompile
>
> - 여러 개의 파일을 하나의 파일로 뭉침
> - cashing으로 저장해두었다가, 다음 페이지 로드시 시간을 줄일 수 있음



- `$ rake assets:clobber`

  - error가 모두 사라진 후 위의 명령어를 통해 compile 된 css들 해체한 후 ,

    `$ rake assets:precompile`을 통해 다시 compile 완료하면 끝







### dashboard.html을 분해 하기  

: index.html.erb에 전부 붙여 넣을 경우, 오류 발생시 해당 줄을 찾기에 번거로움

​	 (Topbar(nav bar) - Body - footer 로 나누기)

- *app/views/shared* 폴더 생성 후 render을 위한 views 파일 만들기
  - *_nav.html.erb*   : <!-- Topbar -->  내용물 모두 넣기
  - _sidebar.html.erb : <!-- Side Header --> 내용물 모두 넣기
  - _footer.html.erb : <!-- footer --> 내용물 모두 넣기
- *app/views/home/index.html.erb*
  - <!-- Options Panel -->, <!-- Page Top-->, <!-- Panel Content-->

<br>

- *app/views/layout/application.html.erb* 에서 `render`을 이용해 다시 조립하기

  ```erb
  <!DOCTYPE html>
  <html>
    <head>
      <meta name="viewport" content="width=device-width, initial-scale=1">
        ...
    </head>
      
    <body class="expand-data panel-data">
      <%= render 'shared/nav' %>
      <%= render 'shared/sidebar' %>
      <%= yield %>
      <%= render 'shared/footer' %>
      <%= yield 'javascript_content' %>
    </body>
  </html>
  ```

  

<br>

### javascript 파일 정돈하기

`    <%= yield 'javascript_content' %>` : 특정 위치로 이동함

```
    <% content_for 'javascript_content' do %>
    
    
    <% end %>
```



<sc>

###  img 관련 조정하기

- 템플릿 파일의 'placeholder' - 'images'  모두 가지고 *app/assets/images* 에 옮겨두기
- views 파일들에서 `.png` `.jpg` 검색해서 수정하기
  - `erb`파일임을 고려해서 루비 태그로 변경해주기 (아래 참조)
    - `src="<%= asset_path 'resource/acti-thmb1.jpg' %>"`
  - css파일에서 img 오류가 날 경우, url(asset-path('resources/header01.jpg'));로 수정하기

<br>

### fonts 관련 조정하기

- 템플릿 파일의 'placeholder' - 'fonts' 에서 'fontawesome~'으로 시작하는 파일 빼고 전부 *public/fonts*파일 만들어서 여기에 옮겨두기 

- *public*은 위치가 자동으로 인식됨

  - 인식되지 않는다면 파일 하나 클릭, 오른쪽 버튼에서 'Search in This Foler' 클릭 후 해당 폰트를 검색하기 

    ```css
    /* src:url --> scr:font-url로 전부 변경하기 */
    ....;src:url("../fonts...");src....
    ```

    

  - `font-awesome` 관련 오류가 난다면 이미 Gem으로 다운로드 한 것을 고려해서 위의 상황에서 관련 내용 삭제하기 (아래 내용)

    ```css
    /*!
     *  Font Awesome 4.7.0 by @davegandy - http://fontawesome.io - @fontawesome
     *  License - http://fontawesome.io/license (Font: SIL OFL 1.1, CSS: MIT License)
     */@font-face{font-family:'FontAwesome';src:url('../fonts/fontawesome-webfont.eot?v=4.7.0'); .... (생략)
    ```



vender.stylesheet에 넣고 불러오기



### 새로운 장 추가하기 (css - js )

1. `rails g scaffold product  `
   - `product.coffe → product.js`로 변경하기
2. css 관련 파일 정돈하기
   - 앞서 설정한 css 외의 다른 css가 사용되는지 확인하지
     - 추가적인 css가 있다면 *vendor/assets/sytlesheets*에 관련 파일을 추가한 후 *app/assets/stylesheets/products.scss*에다 추가하기
     - 추가적인 css가 없다면 *app/assets/stylesheets/home.scss*의 내용을 그대로 *app/assets/stylesheets/products.scss* 이곳에다 복사 붙여넣기
3. JavaScript 관련 파일 정돈하기
   - 앞서 설정한 css 외의 다른 css가 사용되는지 확인하지
     - 추가적인 css가 있다면 *vendor/assets/sytlesheets*에 관련 파일을 추가한 후 *app/assets/stylesheets/products.scss*에다 추가하기
     - 추가적인 css가 없다면 *app/assets/stylesheets/home.scss*의 내용을 그대로 *app/assets/stylesheets/products.scss* 이곳에다 복사 붙여넣기



*app/views/products/show.html.er*

```

```



*config/initializers/assets.rb*

```ruby
Rails.application.config.assets.precompile += %w( home.js
                                                  home.scss
                                                  products.js
                                                  products.scss)
```

- `precomfile` 할 대상을 이곳에서 적어줘야 작동하기 때문에  새로 



# 별점 주기

*ajax 구동 순서*

ajax 작동 - 해당 url로 넘어감 - routes - controller  action 정의 - controller 동작 후 - 해당 액션과 이름이 같은 js 파일이 날아가서 동작함

javascript 먼저 작성하고, ajax 작성하기 (바뀌는 모양을 보면서 )



### setInterval() Method

- 계속 요청하지 말고, 몇 초에 한 번씩만 요청하라

```js
# 3000 -> 몇 초마다 요청할 것인지 적기
setInterval(function(){ alert("Hello"); }, 3000);
```









> precompile = js webpack (번들링)
>
> : cashing : css와 js 파일을 저장을 해두어서 두번째 열 때 부터는 시간이 오래 걸리지 않음
>
> `home-..`로 한 파일로 전체 css, js 따로 각 뭉치고 싶은 파이를 만듦
>
> - respond to do |format|
>   - 요청 방식에 따라 보여줄 것을 결정함
>   - 개발자도구 `content-type`을 통해 확인할 수 있음
>
> 



> 참고자료
>
> - http://guides.rubyonrails.org/asset_pipeline.html