# 「从零开始做购物网站」第34章 实作landingpage页面

> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。

## 目标
> 实作landingpage页面

## 步骤

### Step 0:建立新分支
执行```git checkout -b story25```。

### Step 1:修改app/views/welcome/index.html.erb

打开app/views/welcome/index.html.erb，全部代码如下
```ruby app/views/welcome/index.html.erb
<!-- ↓↓↓ 大图滚动banner 代码块↓↓↓ -->
  <div class="row">
    <div class="carousel fade-carousel slide" data-ride="carousel" data-interval="4000" id="bs-carousel">
      <div class="overlay"></div>

      <!-- 默认三张图列表 -->
      <div class="banner">
        <ul class="carousel-indicators ">
          <li data-target="#bs-carousel" data-slide-to="0" class="active"></li>
          <li data-target="#bs-carousel" data-slide-to="1"></li>
          <li data-target="#bs-carousel" data-slide-to="2"></li>
        </ul>
      </div>

      <!-- 文案部分 -->
      <div class="carousel-inner">
        <div class="item slides ">
          <div class="slide-1"></div>
          <div class="hero row">
            <hgroup>
                <h1>「发现更好生活」</h1>
                <h3>最美好物与你分享</h3>
            </hgroup>
          </div>
        </div>
        <div class="item slides">
          <div class="slide-2"></div>
          <div class="hero">
            <hgroup>
                <h1>「生活从不将就」</h1>
                <h3>面向注重生活品质的你</h3>
            </hgroup>
          </div>
        </div>
        <div class="item slides active">
          <div class="slide-3"></div>
          <div class="hero">
            <hgroup>
                <h1>「让生活更有趣」</h1>
                <h3>分享高品质新奇酷产品和使用体验</h3>
            </hgroup>
          </div>
        </div>

      </div>
    </div>
  </div>
<!-- ↑↑↑ 大图滚动banner 代码块 ↑↑↑ -->


<!-- 中间区域介绍内容 -->
  <div class="row">
    <div class="landing-page-mid">
      <div class="container">
        <div class="row">
          <div class="col-md-12">
            <h3 class="tagline text-center">为了你的有趣生活</h1>
              <p class="col-md-6 col-md-offset-3 text-center tagp">
                「季风」致力于发现和分享具有创造性的产品<br>
                在这里你能找到最新的科技和最酷的设计<br>
                提前感受世界的未来<br>
                就像李荣浩的那首「不将就」 <br>
                改变生活也从「不将就」开始<br>
               「季风」为了你的有趣生活</p>
          </div>
        </div>
      </div>
    </div>
  </div>


<!-- 四季如风区域 -->
<div class="row all-categories-box">
  <div class="container">

    <div class="row">
      <div class="container topspan">
        <div class="row">
          <div class="col-xs-12">
            <h3 class="titspan" align="center">四季如风</h3>
          </div>
        </div>
      </div>
    </div>
    <hr width="10%">

    <div class="row">
        <div class="col-xs-12 col-md-3 text-center product-list-style">
          <%= link_to image_tag("https://ww2.sinaimg.cn/large/006tNc79gy1fg3tjztor4j30m80et0tw.jpg", class: "pproduct-list-style-img img-responsive center-block"), "#" %>
          <br>
          <p><a href="#" >个人护理</a></p>
        </div>
        <div class="col-xs-12 col-md-3 text-center product-list-style">
          <%= link_to image_tag("https://ww4.sinaimg.cn/large/006tNc79gy1fg3tkaavvij30ku0drweo.jpg", class: "pproduct-list-style-img img-responsive center-block"), "#" %>
          <br>
          <p><a href="#" >个人配饰</a></p>
        </div>
        <div class="col-xs-12 col-md-3 text-center product-list-style">
          <%= link_to image_tag("https://ww1.sinaimg.cn/large/006tNc79gy1fg3tkqi274j30m80i73zo.jpg", class: "pproduct-list-style-img img-responsive center-block"), "#" %>
          <br>
          <p><a href="#" >轻奢主义</a></p>
        </div>
        <div class="col-xs-12 col-md-3 text-center product-list-style">
          <%= link_to image_tag("https://ww3.sinaimg.cn/large/006tNc79gy1fg3tl4f3alj30jb0ckacf.jpg", class: "pproduct-list-style-img img-responsive center-block"), "#" %>
          <br>
          <p><a href="#" >个人生活</a></p>
        </div>
    </div>

  </div>
</div>

<!-- 首页精选商品列表 -->
  <div class="row all-product-box">
    <div class="container">

      <div class="container topspan">
        <div class="row">
          <div class="col-xs-12">
            <h3 class="titspan" align="center"><%= link_to("精选产品", products_path) %></h3>
          </div>
        </div>
      </div>
      <hr width="10%">

      <%= render 'common/products' %>

    </div>
  </div>


  <!-- 用户反馈 -->
  <div  id="about" class="sec-about">
    <div class="container">
      <h3 class="tagline" align="center">用户反馈</h3>
        <hr width="10%">

        <div class="row">
            <div class="col-md-12" data-wow-delay="0.2s">
                <div class="carousel slide" data-ride="carousel" id="quote-carousel">
                    <!-- Bottom Carousel Indicators -->
                    <ol class="carousel-indicators">
                        <li data-target="#quote-carousel" data-slide-to="0" class="active" ><img class="img-responsive avata" src="https://ww2.sinaimg.cn/large/006tKfTcgy1fg3o4ylt2dj303k03kmwz.jpg" alt="">
                        </li>
                        <li data-target="#quote-carousel" data-slide-to="1" ><img class="img-responsive avata" src="https://ww3.sinaimg.cn/large/006tKfTcgy1fg3o4s80noj303k03k0sj.jpg" alt="">
                        </li>
                        <li data-target="#quote-carousel" data-slide-to="2"><img class="img-responsive avata" src="https://ww1.sinaimg.cn/large/006tKfTcgy1fg3o4kci06j303k03k0sj.jpg" alt="">
                        </li>
                    </ol>

                    <!-- Carousel Slides / Quotes -->
                    <div class="carousel-inner text-center">

                        <!-- Quote 1 -->
                        <div class="item active">
                            <blockquote>
                                <div class="row">
                                    <div class="col-sm-8 col-sm-offset-2 ">

                                        <p > 这商城老霍先发现的，我的生活用品都在这买啦，哈哈，发现好多好玩的东西，根本停不下来!它不仅是一个购物网站，更是一种生活方式的表达。</p>
                                        <small>胡歌</small>
                                    </div>
                                </div>
                            </blockquote>
                        </div>
                        <!-- Quote 2 -->
                        <div class="item">
                            <blockquote>
                                <div class="row">
                                    <div class="col-sm-8 col-sm-offset-2">

                                        <p>很幸运 季风 请我代言，这是一个分享新奇酷产品和使用体验的社区。你不仅可以通过 季风 发现和分享新奇酷的产品，还可以将你的体验感受与他人分享。在这里你能找到最新的科技和最酷的设计，提前感受世界的未来。 </p>
                                        <small>霍建华</small>
                                    </div>
                                </div>
                            </blockquote>
                        </div>
                        <!-- Quote 3 -->
                        <div class="item">
                            <blockquote>
                                <div class="row">
                                    <div class="col-sm-8 col-sm-offset-2">

                                        <p>我喜欢这里的观点、喜欢这里的酷玩。到了这里真切感觉科技、艺术与生活的融合，上面的每样东西都想买！已将很多非常酷的产品收入囊中!</p>
                                        <small>网红思聪</small>
                                    </div>
                                </div>
                            </blockquote>
                        </div>
                    </div>

                    <!-- Carousel Buttons Next/Prev -->
                    <a data-slide="prev" href="#quote-carousel" class="left carousel-control"><i class="fa fa-chevron-left"></i></a>
                    <a data-slide="next" href="#quote-carousel" class="right carousel-control"><i class="fa fa-chevron-right"></i></a>
                </div>
            </div>
        </div>
    </div>
  </div>


  <!-- 团队介绍版面   -->
  <div class="row landing-page-eva">
    <div class="container">
      <div class="col-md-12">
        <h3 class="tagline">团队介绍</h3>
          <hr width="10%">
          <div class="col-xs-12 col-sm-6 col-md-6 landing-page-eva-style">
            <div class="col-xs-3">
              <%= image_tag("https://ww4.sinaimg.cn/large/006tNbRwgy1fgl4ouucfsj30pm0t0wfi.jpg", class: "img-circle img-responsive img-center frontpage-img") %>
            </div>
            <div class="col-xs-9">
              <h5><a href="http://lll96-blog.logdown.com/" target="_blank">LLL96（胡颖）</a></h5>
              <p> 编程路上新手。<br>
                  喜欢咖啡和泡各种茶、健身爱好者。 <br>
                  喜欢好玩的新鲜事。<br>
                  欣赏江湖义气、话痨爱交朋友。<br></p>
            </div>
          </div>
          <div class="col-xs-12 col-sm-6 col-md-6 landing-page-eva-style">
            <div class="col-xs-3">
              <%= image_tag("https://ww3.sinaimg.cn/large/006tNbRwgy1fgl4pmxbqyj30mi0mjt9k.jpg", class: "img-circle img-responsive img-center frontpage-img") %>
            </div>
            <div class="col-xs-9">
              <h5><a href="http://kerzzi.logdown.com/" target="_blank">柯子(刘铮)</a></h5>
              <p> 爱好编程的辐射防护工程师。 <br>
                  喜欢读书、思考。爱游戏、爱生活。<br>
                  专注个人成长、努力活在未来。<br>
                  欢迎和我一起探索这个世界！<br></p>
            </div>
          </div>
      </div>
    </div>
  </div>


<!-- 公告栏 -->
  <div class="row landing-page-advup">
    <div class="landing-page-adv">
      <div class="container">
        <div class="row">
          <h4>发现最美好物，生活从不将就！</h4>
          <h5>「季风」为了你的有趣生活！</h5>
          <a class="btn btn-primary-welcome " href="products#index" role="button">随风而动</a>
        </div>
      </div>
    </div>
  </div>

```


###Step 2:增加相应样式
打开app/assets/stylesheets/welcome.scss，增加相应样式
```ruby app/assets/stylesheets/welcome.scss
// Place all the styles related to the welcome controller here.
// They will automatically be included in application.css.
// You can use Sass (SCSS) here: http://sass-lang.com/


/* === 以下是首页banner滚动大图的CSS样式包 === */

/* --- 大图高度 和 滚动按钮 --- */
.fade-carousel {                                      //滚动大图的相框高
    position: relative;
    height: 816px;
    margin-top: -15px;
}
.fade-carousel .carousel-inner .item {                //图片截取的宽度，这里与相框高度一致
    height: 816px;
}
.fade-carousel .carousel-indicators > li {            //滚动按钮的设置
    width: 5px;
    height: 5px;
    margin: 0 2px;
    background-color: #fff;
    opacity: 0.5;
}
.fade-carousel .carousel-indicators > li.active {     //当前滚动按钮的设置
    width: 40px;
    height: 5px;
    opacity: 0.8;
}

/*  --- 标题文字设置 ---  */
.hero {
    position: absolute;
    top: 50%;
    left: 50%;
    z-index: 3;                                       //元素堆叠等级3，将显示在最上层
    color: #fff;
    text-align: center;
    text-transform: uppercase;                        //英文大写
    text-shadow: 0 0 5px #666;            //文字阴影 和 动态显示时间
      -webkit-transform: translate3d(-50%,-50%,0);
         -moz-transform: translate3d(-50%,-50%,0);
          -ms-transform: translate3d(-50%,-50%,0);
           -o-transform: translate3d(-50%,-50%,0);
              transform: translate3d(-50%,-50%,0);
}
.hero h1 {                                            //h1标题设置
    font-size: 4em;
    font-weight: 500;
    margin: 10px;
    padding: 0;
}

.hero h3 {                                            //h1标题设置
    letter-spacing: 0.1em;
}

.fade-carousel .carousel-inner .item .hero {          //动态出现设置
    opacity: 0;
    -webkit-transition: 2s all ease-in-out .1s;
       -moz-transition: 2s all ease-in-out .1s;
        -ms-transition: 2s all ease-in-out .1s;
         -o-transition: 2s all ease-in-out .1s;
            transition: 2s all ease-in-out .1s;
}
.fade-carousel .carousel-inner .item.active .hero {   //动态出现设置
    opacity: 1;
    -webkit-transition: 2s all ease-in-out .1s;
       -moz-transition: 2s all ease-in-out .1s;
        -ms-transition: 2s all ease-in-out .1s;
         -o-transition: 2s all ease-in-out .1s;
            transition: 2s all ease-in-out .1s;
}

/*  --- 遮罩滤镜层 ---  */
.overlay {                                            //遮罩滤镜层，把图片调暗，突出文案内容
    position: absolute;
    width: 100%;
    height: 100%;
    z-index: 2;                                       //元素堆叠等级2，将显示在最中层
    background-color: #080d15;
    opacity: 0;
}

/*  --- 按钮 ---  */
.btn.btn-lg {padding: 10px 40px;}
.btn.btn-hero,
.btn.btn-hero:hover,
.btn.btn-hero:focus {
    color: #f5f5f5;
    background-color: #1abc9c;
    border-color: #1abc9c;
    outline: none;
    margin: 20px auto;
}

/*  --- 图片 ---  */
.fade-carousel .slides .slide-1,
.fade-carousel .slides .slide-2,
.fade-carousel .slides .slide-3 {
  height: 816px;
  background-size: cover;                             //确保图片完全撑开
  background-position: center center;
  background-repeat: no-repeat;
}
.fade-carousel .slides .slide-1 {
  background-image: url(https://ww3.sinaimg.cn/large/006tKfTcgy1fg7v5k07bij31kw0w0774.jpg);
}
.fade-carousel .slides .slide-2 {
  background-image: url(https://ww1.sinaimg.cn/large/006tKfTcgy1fg7vwjz7taj31kw0w2dll.jpg);
}
.fade-carousel .slides .slide-3 {

  background-image: url(https://ww3.sinaimg.cn/large/006tKfTcgy1fg7wqj9r2dj31kw0lctcl.jpg);
}

/*  --- 响应式设置 ---  */
@media screen and (min-width: 980px){
    .hero { width: 980px; }
}
@media screen and (max-width: 640px){
    .hero h1 { font-size: 3em; }
}
@media screen and (max-width: 640px){
    .hero h3 { font-size: 1.5em; }
}

/* --- 完成首页banner滚动大图的CSS样式包 --- */


// 中间区域（为了你的有趣生活）介绍内容样式
 .landing-page-mid {                                   //给图片整好形
   height: 770px;
   background-size: cover;                             //确保图片完全撑开
   background-position: center center;
   background-repeat: no-repeat;
   background-image: url(https://ww2.sinaimg.cn/large/006tNc79gy1fg3uatrwy8j31kw0wudoi.jpg);
 }
 .landing-page-mid .tagline {                          //编写标题样式
   text-align: center;
   color: #7f8c8d;
   letter-spacing: 0.1em;                              //把字间距撑开，视觉感更优雅
   font-size: 1.6em;
   margin-top: 3.2em;
 }
 .landing-page-mid .tagp {                             //编写内容样式
   text-align: center;
   color: #666;
   font-size: 1.2em;
   margin-top: 2.5em;
   line-height: 2.5em;                                 //行间距也要拉开大一些
   letter-spacing: 0.18em;                             //把字间距撑开，视觉感更优雅
 }

 // 区块分类标识样式
 .topspan {

   margin-top: 2em;
 }
 .titspan {
   font-weight: 400;
 }
 .morespan {
   margin: 2em auto;
 }

 // 四季如风区域样式设置
 .all-categories-box{
  background-color: #f5f5f5;
  }

 .product-course-box-1 {
   background-color: #7f8c8d;
   padding: 30px 30px;
   color: #fff;
   text-align: center;
   a {
     color: #fff;
   }
   a:hover {
     color: #f5f5f5;
   }
 }
 .product-course-box-2 {
   background-color: #8d827f;
   padding: 30px 30px;
   color: #fff;
   text-align: center;
   a {
     color: #fff;
   }
   a:hover {
     color: #f5f5f5;
   }
 }
 .product-course-box-3 {
   background-color: #8d7f85;
   padding: 30px 30px;
   color: #fff;
   text-align: center;
   a {
     color: #fff;
   }
   a:hover {
     color: #f5f5f5;
   }
 }

 //-------  用户反馈区 -------------//
 .sec-about {
   background-color: #EBEBEB;
   margin: -15px 0 -15px 0;
   .container{
     padding: 30px 0 30px 0;
   }
 }

 .sec-about p {
   text-align: center;
 }
 .sec-about a {
   color: #000;
   &:hover,
   &:focus {
     color: #ad0000;
   }
 }

 /* Carousel with face indicators begin */

  #quote-carousel {
     padding: 0 10px 10px 10px;
     margin-top: 30px;
     /* Control buttons  */
     /* Previous button  */
     /* Next button  */
     /* Changes the position of the indicators */
     /* Changes the color of the indicators */
  }
  #quote-carousel .carousel-control {
     background: none;
     color: #CACACA;
     font-size: 2.3em;
     text-shadow: none;
    //  margin-top: 30px;
  }
  #quote-carousel .carousel-control.left {
     left: -60px;
  }
  #quote-carousel .carousel-control.right {
     right: -60px;
  }
  #quote-carousel .carousel-indicators {
     right: 50%;
     top: auto;
     bottom: 0px;
     margin-right: -19px;
  }
  #quote-carousel .carousel-indicators li {
     width: 50px;
     height: 50px;
     margin: 5px;
     cursor: pointer;
     border: 2px solid #CCC;
     border-radius: 50px;
     opacity: 0.4;
     overflow: hidden;
     transition: all 0.4s;
     .avata {
         width: 100%;
         height: 100%;
         border-radius: 50%;
         margin-top: 0;
     }
  }

  #quote-carousel .carousel-indicators .active {
     background: #333333;
     width: 100px;
     height: 100px;
     border-radius: 100px;
     border-color: #f33;
     opacity: 1;
     overflow: hidden;
  }
  #quote-carousel .carousel-inner {
     min-height: 250px;
  }
  .item blockquote {
     border-left: none;
     margin: 0;
  }
 .item blockquote p:before {
     content: "\f10d";
     font-family: 'Fontawesome';
     float: left;
     margin-right: 10px;
  }


 /* Carousel with face indicators over */
 //-------  end of 用户反馈区 -------------//

 // 精选产品展示列表样式设置，为商品栏做好排版整形
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

 //团队介绍版面样式
 .landing-page-eva {
   background-color: #fff;
   padding-bottom: 6em;
 }
 .landing-page-eva .tagline {                          //编写标题样式
   text-align: center;
  //  color: #7f8c8d;
   letter-spacing: 0.2em;                              //把字间距撑开，视觉感更优雅
   font-size: 1.6em;
   margin-top: 2.8em;
   margin-bottom: 2em;
 }
 .landing-page-eva-style {
   margin-top: 2em;
   text-align: justify;
   line-height: 2em;
 }

 //公告栏样式
 .landing-page-adv {
   padding-bottom: 2em;
   text-align: center;
   background-size: cover;                             //确保图片完全撑开
   background-position: center center;
   background-repeat: no-repeat;
   background-image: url("https://ooo.0o0.ooo/2017/05/13/5915dfb428d47.jpg");
   color: #fff;
 }
 .landing-page-adv h4{
   margin-top: 30px;
   text-align: center;
   font-size: 1.3em;
 }
 .landing-page-adv h5{
   margin-top: 30px;
   text-align: center;
   font-size: 1.8em;
 }

 .btn.btn-primary-welcome{
     position: relative;
     background-color: transparent;
     color: #fff;
     border-color: #fff;
     margin-top: 20px;
     font-size: 20px;
     letter-spacing: 1px;
     z-index:3;
 }

 .btn.btn-primary-welcome:hover{
   background-color: #217F7B;
   border-color: #217F7B;
   color: #fff;
 }

```

### Step 3:修改app/controllers/welcome_controller.rb
打开app/controllers/welcome_controller.rb
```ruby app/controllers/welcome_controller.rb
class WelcomeController < ApplicationController
  def index
    @products = Product.onshelf.page(params[:page] || 1).per_page(params[:per_page] || 16)
                .order("id desc").includes(:main_product_image)
  end
end
```

### Step 4:修改主页路由

打开config/routes.rb
```ruby config/routes.rb
-  root 'products#index'
+  root 'welcome#index'
```

### Step 5:将变更 commit 进去 git 里面。

```
git add .
git commit -m "实作landingpage页面"
git push origin story26
git checkout master
git merge story26
git push origin master
```

## 本章详解
>  努力更新中，上班族请理解。。。。。
