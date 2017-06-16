# 「从零开始做购物网站」第19章 暂停使用carrierwave，选用paperclip进行多图上传

> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。


## 目标
> 暂停使用carrierwave，选用paperclip进行多图上传


## 步骤
### Step 0:建立新分支
在终端执行```git checkout -b story13```。

### Step 1:安装gem 'paperclip'

打开Gemfile
```ruby Gemfile
  gem 'ancestry'
  gem 'will_paginate'
+ gem 'paperclip'
```

检查系统中是否已安装ImageMagick，在终端中输入：```which convert```，如果有输出路径，比如```/usr/local/bin/convert```。

说明已经安装了，如果没有安装,请先安装ImageMagick。


执行```bundle install```。


### Step 2:创建商品图片model

在终端执行```rails g model product_image```。

### Step 3:创建图片的关联关系：
打开新创建migration文件，指定几个字段：
```ruby db/migrate/20170610060029_create_product_images.rb
class CreateProductImages < ActiveRecord::Migration[5.0]
  def change
    create_table :product_images do |t|
 +    t.belongs_to :product
 +    t.integer :weight, default: 0
 +    t.attachment :image
      t.timestamps
    end
  end
end
```


在终端执行```rails db:migrate```。

打开schema.rb可以查看生成了哪些字段
中间以image开头的4个字段，是```t.attachment :image```语句自动生成的。

```ruby config/schema.rb
  create_table "product_images", force: :cascade do |t|
    t.integer  "product_id"
    t.integer  "weight",             default: 0
    t.string   "image_file_name"
    t.string   "image_content_type"
    t.integer  "image_file_size"
    t.datetime "image_updated_at"
    t.datetime "created_at",                     null: false
    t.datetime "updated_at",                     null: false
    t.index ["product_id"], name: "index_product_images_on_product_id"
  end
```

### Step 4:修改app/model/product_image.rb

打开app/model/product_image.rb
```ruby 打开app/model/product_image.rb
class ProductImage < ApplicationRecord

  belongs_to :product

  #指定图片尺寸
  has_attached_file :image, styles: {
    small: '60^x60',
    middle: '200^x200',
    big: "960x"
  }

  #限制上传类型
  validates_attachment_content_type :image, content_type: /\Aimage\/.*\Z/
  #限制上传图片的大小
  validates_attachment_size :image, in: 0..5.megabytes

end
```

### Step 5:修改app/models/product.rb

打开app/models/product.rb，加入
```ruby app/models/product.rb
+  has_many :product_images, -> { order(weight: 'desc') }, dependent: :destroy
```

### Step 6:商品图片上传和删除功能开发

打开打开routes.rb,为图片管理创建路由
```ruby config/routes.rb
Rails.application.routes.draw do
  root 'welcome#index'

  resources :users
  resources :sessions

  delete '/logout' => 'sessions#destroy', as: :logout

  namespace :admin do
    root 'sessions#new'
    resources :sessions
    resources :categories
 -  resources :products
 +   resources :products do
 +     resources :product_images, only: [:index, :create, :destroy, :update]
 +   end
  end
end
```
虽然不指定动作也是可以的，但是这样做性能会提升，减少路由批评和查找的资源和时间消耗。
### Step 7:

在终端执行```rails g controller admin::product_images```。

这里之所以不将商品上传放在商品建立时的页面，是因为一旦商品建立时，图片已经上传完毕（上传需要一定的时间），但是如果商品信息不正确等原因可能导致上传失败，从而造成资源、性能和时间上的浪费，给后台的商品管理带来不便。因此我们将**图片上传**和**商品新建**完全分开设计。我们把商品图片作为商品的一个属性来独立管理。

查看商品管理的路由：

> 这里分享一个小知识：输出路由过滤，如仅输出image相关的路由。
> 执行```rails routes | grep image```。, 输出结果

```
admin_product_product_images GET    /admin/products/:product_id/product_images(.:format)     admin/product_images#index
                             POST   /admin/products/:product_id/product_images(.:format)     admin/product_images#create
 admin_product_product_image PATCH  /admin/products/:product_id/product_images/:id(.:format) admin/product_images#update
                             PUT    /admin/products/:product_id/product_images/:id(.:format) admin/product_images#update
                             DELETE /admin/products/:product_id/product_images/:id(.:format) admin/product_images#destroy
```


打开app/views/admin/products/index.html.erb,加入商品图片管理按钮

```ruby app/views/admin/products/index.html.erb
    <td align="right">
      <%= link_to "编辑", edit_admin_product_path(product) %>
      <%= link_to "删除", admin_product_path(product), method: :delete, 'data-confirm': "确认删除吗?" %>
+         <%= link_to "图片管理", admin_product_product_images_path(product_id: product) %>
    </td>
```

### Step 8:修改admin/product_images_controller.rb

前往admin/product_images_controller.rb
```ruby app/controller/admin/product_images_controller.rb
class Admin::ProductImagesController < Admin::BaseController

  before_action :find_product

  def index
    @product = Product.find params[:product_id]
+   @product_images = @product.product_images
  end

end
```


### Step 9:建立商品管理的页面
在终端执行```touch app/views/admin/product_images/index.html.erb```。

```ruby app/views/admin/product_images/index.html.erb
<div>
  <div class="pull-right">
    <%= form_tag admin_product_product_images_path(product_id: @product), method: :post, enctype: "multipart/form-data", class: "form-horizontal" do %>
      <input type="file" name="images[]" multiple class="image-input" />
      <%= submit_tag "上传", class: "btn btn-primary" %>
    <% end -%>
  </div>

  <h1>
    商品 #<%= @product.id %> <%= @product.title %>
  </h1>
</div>

<div>
  <table class="table table-striped">
    <tr>
      <th>ID</th>
      <th>图片</th>
      <th>权重</th>
      <th></th>
    </tr>

    <% @product_images.each do |product_image| %>
      <tr>
        <td><%= product_image.id %></td>
        <td><%= image_tag product_image.image.url(:middle) %></td>
        <td>
          <%= form_tag admin_product_product_image_path(@product, product_image), method: :put do %>
            <input type="text" value="<%= product_image.weight %>" name="weight" />
            <%= submit_tag "更新", class: "btn btn-default btn-xs" %>
          <% end -%>
        </td>
        <td align="right">
          <%= link_to "删除", admin_product_product_image_path(@product, product_image), method: :delete, 'data-confirm': "确认删除吗?" %>
        </td>
      </tr>
    <% end -%>
  </table>

</div>
```

打开products.scss文件，加入一个小样式（你可以自己修改）
```ruby products.scss
 // 图片上传区块样式
input[type="file"] {
  &.image-input {
    display: inline-block;
    border: 1px solid #ccc;
    padding: 5px 20px;
    border-radius: 4px
  }
}
```

### Step 10:修改app/controller/admin/product_images_controller.rb
打开app/controller/admin/product_images_controller.rb
```ruby app/controller/admin/product_images_controller.rb
class Admin::ProductImagesController < Admin::BaseController

  before_action :find_product

  def index
    @product_images = @product.product_images
  end

  def create
    params[:images].each do |image|
      @product.product_images << ProductImage.new(image: image)
    end

    redirect_to :back
  end

  def destroy
    @product_image = @product.product_images.find(params[:id])
    if @product_image.destroy
      flash[:notice] = "删除成功"
    else
      flash[:notice] = "删除失败"
    end

    redirect_to :back
  end

  def update
    # 取得当前的图片
    @product_image = @product.product_images.find(params[:id])
    # 取得当前图片的权重
    @product_image.weight = params[:weight]
    if @product_image.save
      flash[:notice] = "修改成功"
    else
      flash[:notice] = "修改失败"
    end

    redirect_to :back
  end

  private
  def find_product
    @product = Product.find params[:product_id]
  end

end
```

正常应该先找到商品，再找该商品的图片,我们上边的代码用了重构。


### Step 11:商品图片的权重功能开发
给商品图片建立索引。
在终端执行 ```rails g migration add_product_images_index```。

打开db/migrate/20170610062347_add_product_images_index.rb，建立2个字段的联合索引
```ruby db/migrate/20170610062347_add_product_images_index.rb
class AddProductImagesIndex < ActiveRecord::Migration[5.0]
  def change
+   add_index :product_images, [:product_id, :weight]
  end
end
```

2个字段的联合索引

在终端执行```rails db:migrate```。

将路径public/system/product_images，加入.gitignore文件
```ruby .gitignore
# Ignore Byebug command history file.
  .byebug_history
  config/database.yml
  public/uploads
+ public/system/product_images
```

### Step 12:将变更 commit 进去 git 里面。

```
git add .
git commit -m "完成独立的商品图片管理"
git push origin story13
git checkout master
git merge story13
git push origin master
```


## 本章详解
>  努力更新中，上班族请理解。。。。。
