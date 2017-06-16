# 「从零开始做购物网站」第24章 使用SendCloud服务发送邮件

> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。


## 目标
>  本节是全栈营JDstore教程“使用SendCloud服务发送邮件”，如果您已经在前11章做过这节，请勿重做，跳过本节即可。

## 步骤

### Step 0:建立新分支
在终端执行```git checkout -b story17-1```。

### Step 1:
在开始之前，先将之前的一个小BUG，修改修复一下：
打开app/views/carts/index.html.erb，给购物车页面增加显示图片
```ruby app/views/carts/index.html.erb
              <%= link_to product_path(cart_item.product) do %>
 -              <% if cart_item.product.image.present? %>
 +              <% if cart_item.product.main_product_image.present? %>
 -                 <%= image_tag(cart_item.product.image.thumb.url, class: "thumbnail") %>
 +                 <%= image_tag(cart_item.product.main_product_image.image.url(:small), class: "thumbnail") %>
                <% else %>
```

### Step 2:
注册SendCloud

http://sendcloud.net/

将生成的API_USER和API_KEY信息保存下来

如果忘记 API_KEY，可以到 发送设置 生成一个新的


### Step 3:修改文件

修改开发环境用于本地测试

```ruby config/environments/development.rb
Rails.application.configure do
...(略)
－    config.action_mailer.delivery_method = :letter_opener
＋  # config.action_mailer.delivery_method = :letter_opener

＋  config.action_mailer.delivery_method = :smtp
＋  ActionMailer::Base.smtp_settings = {
＋    address: "smtpcloud.sohu.com",
＋    port: 25,
＋    domain: "heroku.com",
＋    authentication: "login",
＋    enable_starttls_auto: true,
＋    user_name: ENV["SEND_CLOUD_USER_NAME"],
＋    password: ENV["SEND_CLOUD_USER_KEY"]
＋    }
end
```


修改生产环境用于heroku使用

```ruby config/environments/production.rb
Rails.application.configure do
...(略)

+  config.action_mailer.default_url_options = { :host => '你的herokuapp地址'}

+  config.action_mailer.delivery_method = :smtp
+  ActionMailer::Base.smtp_settings = {
+    address: "smtpcloud.sohu.com",
+    port: 25,
+    domain: "heroku.com",
+    authentication: "login",
+    enable_starttls_auto: true,
+    user_name: ENV["SEND_CLOUD_USER_NAME"],
+    password: ENV["SEND_CLOUD_USER_KEY"]
+    }
end
```

```ruby config/application.yml
production:
  AWS_ACCESS_KEY_ID: "xxxx"
  AWS_SECRET_ACCESS_KEY: "xxxx"
  AWS_REGION: "xxxx"
  AWS_BUCKET_NAME: "xxxx"
+ SEND_CLOUD_USER_NAME: "xxxx"  # 你的 API_USER

+ SEND_CLOUD_USER_KEY: "xxxx"  # 你的 API_KEY



development:
  AWS_ACCESS_KEY_ID: "xxxx"
  AWS_SECRET_ACCESS_KEY: "xxxx"
  AWS_REGION: "xxxx"
  AWS_BUCKET_NAME: "xxxx"
+ SEND_CLOUD_USER_NAME: "xxxx"  # 你的 API_USER

+ SEND_CLOUD_USER_KEY: "xxxx"  # 你的 API_KEY
```
### Step 4: 本地测试

重开```rails server```。

请用真实邮箱注册登录你的网站，生成订单后查收注册邮箱，收到邮件即可确认成功

如果出现下图报错，请断开VPN再测试


### Step 5:部署到heroku

```
git add .
git commit -m "sendcloud settings"
figaro heroku:set -e production
git push origin story-1
git checkout master
git merge story17-1
git push origin master
git push heroku
heroku pg:reset DATABASE
heroku run rails db:migrate
heroku run rails db:seed
```

同样请用真实邮箱注册登录你的网站，生成订单后查收注册邮箱，收到邮件即可确认成功

## 本章详解
>  努力更新中，上班族请理解。。。。。
