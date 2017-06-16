# 「从零开始做购物网站」第33章 美化navbar、footer、商品首页

> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。

## 目标
> 美化navbar、footer、商品首页

## 步骤

### Step 0:建立新分支
执行```git checkout -b story25```。

### Step 1:

修改navbar内容，做如下替换
```ruby app/views/common/_navbar.html.erb
<nav class="navbar navbar-default navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <a class="navbar-brand" href="/"><strong> MONSOON </strong></a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
             <li>
               <%= link_to("商品", products_path) %>
             </li>
             <li class="dropdown">
               <a class="dropdown-toggle" data-toggle="dropdown" href="#">分类浏览
                 <span class="caret"></span>
               </a>
               <ul class="dropdown-menu">
                 <% @categories.each do |group| %>
                   <% group.last.each do |sub_category| %>
                     <li class="list-group-item"><a href="<%= category_path(sub_category) %>"><%= sub_category.title %></a></li>
                   <% end %>
                 <% end %>
               </ul>
             </li>
             <li><%= link_to "服务", static_pages_help_path %></li>
             <li><%= link_to "关于", static_pages_about_path %></li>
            </ul>
            <!-- 搜索框 -->
            <%= form_tag search_products_path, method: :get, class: "navbar-form navbar-left" do %>
              <div class="form-group">
                <% if @category %>
                  <input type="hidden" name="category_id" value="<%= @category.id %>" />
               <% end %>
               <input type="text" name="w" value="<%= params[:w] %>" class="form-control search-input" placeholder="<%= @category ? "在 #{@category.title} 下搜索.." : '搜索整站商品..' %>">

              </div>
              <button type="submit" class="btn btn-default">搜索</button>
            <% end %>

            <ul class="nav navbar-nav navbar-right">
               <li>
                   <%= link_to carts_path do  %>
                      购物车 <i class="fa fa-shopping-cart"> </i> ( <%= current_cart.products.count %> )
                   <% end %>
               </li>
                <% if !current_user %>
                  <li><a href="#" data-toggle="modal" data-target="#signup-modal">注册</a></li>
                  <li><a href="#" data-toggle="modal" data-target="#login-modal">登入</a></li>
                <% else %>
                  <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown">
                        Hi!, <%= current_user.email %>
                        <b class="caret"></b>
                    </a>
                    <ul class="dropdown-menu">
                         <% if current_user.admin? %>
                           <li>
                              <%= link_to('后台管理<span class="glyphicon glyphicon-stats pull-right">'.html_safe,
                                         admin_root_path) %>
                           </li>
                           <li role="separator" class="divider"></li>
                           <li> <%= link_to('我的订单 <span class="glyphicon glyphicon-envelope pull-right"></span>'.html_safe, account_orders_path) %> </li>
                           <li> <%= link_to('我的收货地址<span class="glyphicon glyphicon-home pull-right"></span>'.html_safe,"#") %> </li>
                           <li> <%= link_to('我收藏的商品<span class="glyphicon glyphicon-heart pull-right"></span>'.html_safe, "#") %> </li>
                           <li role="separator" class="divider"></li>
                           <li><%= link_to('用户中心 <span class="glyphicon glyphicon-cog pull-right"></span>'.html_safe, edit_account_user_path(current_user)) %></li>
                           <li role="separator" class="divider"></li>
                         <% end %>
                        <li> <%= link_to(content_tag(:i, '登出', class: 'fa fa-sign-out'), destroy_user_session_path, method: :delete) %> </li>
                    </ul>
                  </li>
                <% end %>
            </ul>
        </div>
        <!-- /.navbar-collapse -->
    </div>
    <!-- /.container-fluid -->
</nav>

<!-- 以下代码实现登入登出的弹窗效果 -->
<div class="modal fade" id="login-modal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true" style="display: none;">
  <div class="modal-dialog" role="document">
    <div class="loginmodal-container">
       <h2>Log in</h2>
       <%= simple_form_for(resource, as: resource_name, url: session_path(resource_name)) do |f| %>
         <div class="form-inputs">
           <%= f.input :email, required: false, autofocus: true %>
           <%= f.input :password, required: false %>
           <%= f.input :remember_me, as: :boolean if devise_mapping.rememberable? %>
         </div>
         <div class="form-actions">
           <%= f.button :submit, "Log in" %>
         </div>
       <% end %>
    </div>
  </div>
</div>

<div class="modal fade" id="signup-modal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true" style="display: none;">
  <div class="modal-dialog">
    <div class="loginmodal-container">
      <h2>Sign up</h2>
        <%= simple_form_for(resource, as: resource_name, url: registration_path(resource_name)) do |f| %>
        <%= f.error_notification %>
        <div class="form-inputs">
          <%= f.input :email, required: true, autofocus: true %>
          <%= f.input :password, required: true, hint: ("#{@minimum_password_length} characters minimum" if @minimum_password_length) %>
          <%= f.input :password_confirmation, required: true %>
        </div>
        <div class="form-actions">
            <%= f.button :submit, "Sign up" %>
        </div>
      <% end %>
    </div>
  </div>
</div>

```

修改navbar样式,加入如下内容
```ruby app/assets/stylesheets/application.scss
/*===== navbar ====*/

 .navbar-default {

   margin-bottom: 0;
   border-radius: 0;
   background: #000;

     color: #000 !important;
     font-size: 1.2em;

     a:focus {
       color: #eb5424 !important;
     }
     &.show_bgcolor {
       background-color: #02061A;
     }

     .navbar-header {
       a {
         font-size: 20px;
         color: #fff;
         transition: 0.35s;
       }
       a:hover {
         color: #D0CFCE;
       }
     }
     .navbar-nav li a {
       color: #fff;
       font-size: 16px;
       transition: 0.35s;
     }
     .navbar-nav li a:hover {
       color: #D0CFCE;
       text-decoration: none
     }

     .navbar-nav .dropdown-menu {
       padding-bottom: 0;
       padding-top: 0;
       a {
         text-align: center;
         color: #000;
       }
       li.divider:last-child {
         display: none;
       }
     }
     .login {
       border: 1px solid #fff;
       margin-top: 12px;
       padding: 3px 17px 3px 17px;
       margin-right: 130px;
       border-radius: 15px;
     }
     .search-nav{
       width: 280px;
     }

  }

  .navbar-brand {
    letter-spacing: 0.1em;       //拉开一点点字间距
    font-weight:lighter;         //字体粗细比默认样式缩小100磅值，让logo显得精致高级
  }
 /*===== end of navbar ====*/
```



修改footer内容，做如下替换
```ruby app/views/common/_footer.html.erb
<footer class="footer">
	<p><a href="<%= static_pages_help_path %>" target="_blank">关于我们 </a> <span>|
  </span> <a href="<%= static_pages_help_path %>" target="_blank">服务中心</a> <span>|
  </span> <a href="http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon" target="_blank">功能展示</a> <span>|
  </span> <a href="http://kerzzi.logdown.com/posts/1890176" target="_blank">制作教程</a> <span>|
  </span> <a href="http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon" target="_blank">联系我们</a> <span>|
  </span> <a href="https://fullstack.xinshengdaxue.com/works/604" target="_blank">支持我们</a> <span></p>
	<p>
    <span class="logo-text">Copyright ©2017 Monsoon</span>
    <span class="icp-center">Designed By <a href="http://kerzzi.logdown.com/" target="_blank">Kerzzi</a> & <a href="http://lll96-blog.logdown.com/" target="_blank">胡颖</a> </span>
	</p>
</footer>
```

修改navbar样式,加入如下内容
```ruby app/assets/stylesheets/application.scss
//---- footer ------

.footer{
    text-align: center;
    min-height: 50px;
    background-color:  #000;
    // margin-bottom: -50px;
    padding: 20px 0;
    color: #fff;
    p{
      font-size: 14px;
      a{
        // color: #4A6BB5;
        text-decoration:none;
        color: #fff;
      }
      a:hover{
        color: #35CCC6;
      }
      a:focus{
        color: #4A6BB5;
      }
    }
}

/*===== end of footer ====*/
```

由于我们目前商品较少，所以footer目前跑在了屏幕中间，看起来很丑。我们需要将footer置底。

将footer置底

打开app/views/layouts/application.html.erb
```ruby app/views/layouts/application.html.erb
<!DOCTYPE html>
<html>
    <head>
        <title>Monsoon </title>
        <%= csrf_meta_tags %>

        <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
        <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
    </head>

    <body>
      <div id="id_wrapper">
        <div id="id_header">
          <%= render "common/navbar" %>
        </div> <!-- /id_header -->
        <div class="row" id="id_content">
            <div class="container">
              <%= render "common/flashes" %>
            </div>
            <%= yield %>
        </div><!-- /id_content -->

      </div> <!-- /id_wrapper -->
        <div id="id_footer">
          <%= render "common/footer" %>
        </div> <!-- /id_footer -->

    </body>
</html>
```

打开app/assets/stylesheets/application.scss，增加footer置底的样式。
```ruby app/assets/stylesheets/application.scss
 @import url('https://fonts.googleapis.com/earlyaccess/notosanssc.css');   //引入google思源黑简在线字体 需要使用VPN才可生效


 // 界面基础设置
html, body {
    /* 設定body高度為100% 拉到視窗可視的大小 */
    height: 100%;
}

body {

    background: #EBEBEB;
    // background-image: url(https://ww1.sinaimg.cn/large/006tKfTcgy1fg7w2lnv7lj30ud0g8mxo.jpg);
    font-family: 'Noto Sans SC', sans-serif;            // google思源黑简作为主字体
    color: #666;                                        // 修改全站的 字体主色
    // font-size: 1.2em;                                   // 网站所有字体缩小比例
    .breadcrumb{
    background: #fff;
    height: 50px;
    li{
      padding-top: 7px;
    }

    }
   a {                                                 // 所有a标签统一样式
     color: #666;                                      // 链接反转的样式
     transition: 0.5s;                                 // 鼠标移开时 0.5秒渐变
   }
   a:hover {
     color: #7f8c8d;
     transition: 0.5s;                                 // 鼠标指向时 0.5面渐变
     text-decoration: none;                            // 移除下划线
   }
 }

#id_wrapper {
    /* 設定高度最小為100%, 如果內容區塊很多, 可以長大 */
    min-height: 100%;
    /* 位置設為relative, 作為footer區塊位置的參考 */
    // position: relative;
}

#id_header {
    /* 設定header的高度 */
    height: 40px;
    box-sizing: border-box;
}

#id_content {
    /* 留出header及footer區塊的空間 */
    padding-top: 25px;
    // padding-bottom: 60px;
}

#id_footer {
    /* 設定footer的高度 */
    height: 40px;
    box-sizing: border-box;
    /* 設定footer絕對位置在底部 */
    // position: absolute;
    bottom: 0;
    /* 展開footer寬度 */
    width: 100%;
}
```

rails s

###Step 2:
打开app/views/products/index.html.erb，全部代码修改如下
```ruby app/views/products/index.html.erb
<div class="product-box">

  <div class="container ">
    <!--=== 轮播图 ===-->
    <div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
      <ol class="carousel-indicators">
        <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
        <li data-target="#carousel-example-generic" data-slide-to="1"></li>
        <li data-target="#carousel-example-generic" data-slide-to="2"></li>
      </ol>
      <div id="hover" class="carousel-inner" role="listbox">

        <div class="item active" data-slide-to="0">
          <div class="carousel-BC1">
            <div class="container">
              <img src="https://ww3.sinaimg.cn/large/006tKfTcgy1fg7v5k07bij31kw0w0774.jpg" width="1140" alt="...">
              <div class="carousel-caption">
                <p></p>
              </div>
            </div>
          </div>
        </div>
        <div class="item" data-slide-to="1">
          <div class="carousel-BC2">
            <div class="container">
              <img src="https://ww3.sinaimg.cn/large/006tKfTcgy1fg7v6t9ga3j31kw0re1ip.jpg" width="1140" alt="...">
              <div class="carousel-caption">
                <p></p>
              </div>
            </div>
          </div>
        </div>
        <div class="item" data-slide-to="2">
          <div class="carousel-BC3">
            <div class="container">
              <img src="https://ww4.sinaimg.cn/large/006tKfTcgy1fg7vtssvrlj31kw0w0wkq.jpg" width="1140" alt="...">
              <div class="carousel-caption">
                <p></p>
              </div>
            </div>
          </div>
        </div>

      </div>
    </div>


      <ol class="breadcrumb">
        <li class="active">所有商品</li>
      </ol>

      <%= render 'common/products' %>

    </div>
    <div  align="center">
      <%= will_paginate @products %>
    </div>
  </div>
</div>
```

打开app/assets/stylesheets/products.scss，增加相应样式
```ruby app/assets/stylesheets/products.scss
-/****** 商品列表简单样式 ******/

 /*  products index page */
 .product-box .carousel img {
     width: 100%;
     height: 500px;
     margin: 0px 0px 15px -15px;
     border-radius: 10px;
 }

 .product-list-style {                                 //图片+文字 块样式整形
  margin: 3em auto 2em;
  img{
    width: 100%;
    height: 200px;
    border-radius: 10px;
  }
}
.product-list-style-img {                             //文字 块样式整形
  margin-bottom: 1.2em;
}
.all-product-box {
  background-color: #fff;
  margin-bottom: 3em;
}
```

### Step 3:将变更 commit 进去 git 里面。

```
git add .
git commit -m "美化navbar、footer、商品首页"
git push origin story25
git checkout master
git merge story25
git push origin master
```

## 本章详解
>  努力更新中，上班族请理解。。。。。
