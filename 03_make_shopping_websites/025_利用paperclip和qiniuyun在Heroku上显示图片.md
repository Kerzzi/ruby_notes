# 「从零开始做购物网站」第25章 利用paperclip和qiniuyun在Heroku上显示图片

> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。


## 目标
> 利用paperclip和qiniuyun在Heroku上显示图片
> 本功能感谢@刘志新老师的指导，我比赛过程中在这个功能上卡住了2天，刘志新老师远程指导我解决了BUG，非常感谢！

## 步骤
### Step 0:建立新分支
在终端执行```git checkout -b story17-2```。

### Step 1:为什么要使用七牛云存储图片
目前上传至Heroku的图片，可以显示出来。但是如果你再次```git push heroku```后，图片会挂掉。

为了解决这个问题，我们还是选择使用七牛云来存储图片。与carrier一样，使用之前我们需要做一些设置。

### Step 2:安装paperclip-qiniu套件
谷歌之后，选择[gem 'paperclip-qiniu'](https://github.com/lidaobing/paperclip-qiniu)来完成我们的功能。

打开Gemfile，加入paperclip-qiniu套件
```ruby Gemfile
gem 'paperclip-qiniu'
# or get the latest version
# gem 'paperclip-qiniu', :git => "git://github.com/lidaobing/paperclip-qiniu"
```

 执行``` bundle install```。

 执行```bundle update```。

### Step 3:新增config/initializers/paperclip.rb，进行相应的配置

执行```touch config/initializers/paperclip.rb```。

打开config/initializers/paperclip.rb
```ruby config/initializers/paperclip.rb
Paperclip::Attachment.default_options[:storage] = :qiniu
Paperclip::Attachment.default_options[:qiniu_credentials] = {
  :access_key => ENV['QINIU_ACCESS_KEY'] || raise("set env QINIU_ACCESS_KEY"),
  :secret_key => ENV['QINIU_SECRET_KEY'] || raise("set env QINIU_SECRET_KEY")
}
Paperclip::Attachment.default_options[:bucket] = 'monsoon'
Paperclip::Attachment.default_options[:use_timestamp] = false
Paperclip::Attachment.default_options[:qiniu_host] =
  'http://or4mngzht.bkt.clouddn.com'
```

### Step 4:
修改config/application.yml中内容：增加QINIU_ACCESS_KEY和QINIU_SECRET_KEY，该文件完整代码如下。
```ruby config/application.yml
production:
  qiniu_access_key: xxxx  # 你的 qiniu AccessKey

  qiniu_secret_key: xxxx  # 你的 qiniu SecretKey

  qiniu_bucket: xxxx   # 你的 qiniu bucket

  qiniu_bucket_domain: xxxx   # 你的 qiniu bucket domain

  SEND_CLOUD_USER_NAME: "xxxx"  # 你的 API_USER

  SEND_CLOUD_USER_KEY: "xxxx"  # 你的 API_KEY

  QINIU_ACCESS_KEY: xxxx  # 你的 qiniu AccessKey

  QINIU_SECRET_KEY: xxxx  #你的 qiniu SecretKey

development:
  qiniu_access_key: xxxx   # 你的 qiniu AccessKey

  qiniu_secret_key: xxxx   # 你的 qiniu SecretKey

  qiniu_bucket: xxxx   # 你的 qiniu bucket

  qiniu_bucket_domain: xxxx   # 你的 qiniu bucket domain

  SEND_CLOUD_USER_NAME: "xxxx"  # 你的 API_USER

  SEND_CLOUD_USER_KEY: "xxxx"  # 你的 API_KEY

  QINIU_ACCESS_KEY: xxxx   # 你的 qiniu AccessKey

  QINIU_SECRET_KEY: xxxx   # 你的 qiniu SecretKey
```

### Step 5:修改app/models/product_image.rb
打开app/models/product_image.rb，将代码修改为如下内容
```ruby app/models/product_image.rb
class ProductImage < ApplicationRecord

  belongs_to :product

  #指定图片存储的路径规则和图片尺寸
  has_attached_file :image,:path => ":class/:attachment/:id/:basename.:extension"

  validates :image, :attachment_presence => true

  #限制上传类型
  validates_attachment_content_type :image, :content_type => /\Aimage\/.*\Z/

end
```


### Step 6:修改显示图片的几个页面
如果你按照https://github.com/lidaobing/paperclip-qiniu中的方法可以显示图片，那就按照其中的方法操作。即用如下方式替换显示图片的网址。
```
<%= qiniu_image_tag @image.file.url, :thumbnail => '300x300>', :quality => 80 %>
or
<%= image_tag qiniu_image_path(@image.file.url, :thumbnail => '300x300>', :quality => 80) %>
```

但是我按照上面的两种方法都失败了，于是在@刘志新老师的指导下，用了一个笨方法，但是非常实用。具体是这样操作的。

#### 打开app/views/admin/product_images/index.html.erb，做如下修改：
```ruby app/views/admin/product_images/index.html.erb
     <% @product_images.each do |product_image| %>
        <tr>
          <td><%= product_image.id %></td>
 -        <td><%= image_tag product_image.image.url(:middle) %></td>
 +        <td><img src=<%= qiniu_image_path(product_image.image.url) %>></td>
          <td>
```

#### 打开app/views/carts/index.html.erb，做如下修改：
```ruby app/views/carts/index.html.erb
               <%= link_to product_path(cart_item.product) do %>
                <% if cart_item.product.main_product_image.present? %>
-                        <%= image_tag(cart_item.product.main_product_image.image.url(:small), class: "thumbnail") %>
+                        <img src=<%= qiniu_image_path(cart_item.product.main_product_image.image.url) %>>
            <% else %>
```

#### 打开app/views/common/_products.html.erb，做如下修改：
```ruby app/views/common/_products.html.erb
         <%= link_to product_path(product) do %>
            <% if product.product_images.present? %>
 -            <%= link_to image_tag(product.main_product_image.image.url(:middle), alt: product.title), product_path(product) %>
 +            <%= link_to product_path(product) do %>

 +             <img src=<%= qiniu_image_path(product.main_product_image.image.url) %>>
 +            <% end %>
            <% else %>
```

#### 打开app/views/products/show.html.erb，做如下修改：
```ruby app/views/products/show.html.erb
         <div class="col-xs-6 col-md-6">
            <a href="#" class="thumbnail detail">
 -            <%= image_tag product_image.image.url %>
 +            <img src=<%= qiniu_image_path(product_image.image.url) %>>
            </a>
          </div>
        <% end %>
        ......
        ......
                <div class="row">
                   <% @product.product_images.each do |product_image| %>
                     <div class="col-xs-12 col-md-12 product-show-detail">
 -                       <%= image_tag product_image.image.url(:big) %>
 +                       <img src=<%= qiniu_image_path(product_image.image.url) %>>
                     </div>
                   <% end %>
```

在终端执行```rake db:reset```，清空之前的数据。

重启```rails s```。

### Step 7:将变更 commit 进去 git 里面。

```
git add .
git commit -m "使用paperclip和七牛云存储图片"
git push origin story17-2
git checkout master
git merge story17-2
git push origin master
```

## 本章详解
>  努力更新中，上班族请理解。。。。。
