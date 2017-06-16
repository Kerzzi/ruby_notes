# 「从零开始做购物网站」第18章 优化商品管理：增加部分属性,合并new和edit页面

> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。


## 目标
> 优化商品管理：增加部分属性,合并new和edit页面


## 步骤
### Step 0:建立新分支
在终端执行```git checkout -b story12```。

### Step 1:修改app/controllers/admin/products_controller.rb

打开app/controllers/admin/products_controller.rb，完整代码如下：
```ruby app/controllers/admin/products_controller.rb
class Admin::ProductsController < Admin::BaseController

  before_action :find_product, only: [:edit, :update, :destroy]

  def index
   @products = Product.page(params[:page] || 1).per_page(params[:per_page] || 10)
      .order("id desc")
  end

  def new
    @product = Product.new
    @root_categories = Category.roots
  end

  def edit
    @root_categories = Category.roots
    render action: :new
  end

  def create
    @product = Product.new(params.require(:product).permit!)
    @root_categories = Category.roots

    if @product.save
      flash[:notice] = "创建成功"
      redirect_to admin_products_path
    else
      render action: :new
    end
  end

  def update
    @product.attributes = params.require(:product).permit!
    @root_categories = Category.roots
    if @product.save
      flash[:notice] = "修改成功"
      redirect_to admin_products_path
    else
      render action: :new
    end
  end

  def destroy
    if @product.destroy
      flash[:notice] = "删除成功"
      redirect_to admin_products_path
    else
      flash[:notice] = "删除失败"
      redirect_to :back
    end
  end

  private

  def find_product
    @product = Product.find(params[:id])
  end
end
```

### Step 2:修改app/models/product.rb

打开app/models/product.rb，完整代码如下
```ruby app/models/product.rb
class Product < ApplicationRecord

  mount_uploader :image, ImageUploader

  validates :category_id, presence: { message: "分类不能为空" }
  validates :title, presence: { message: "名称不能为空" }
  validates :status, inclusion: { in: %w[on off],
    message: "商品状态必须为on | off" }
  validates :quantity, numericality: { only_integer: true,
    message: "库存必须为整数" },
    if: proc { |product| !product.quantity.blank? }
  validates :quantity, presence: { message: "库存不能为空" }
  validates :msrp, presence: { message: "MSRP不能为空" }
  validates :msrp, numericality: { message: "MSRP必须为数字" },
    if: proc { |product| !product.msrp.blank? }
  validates :price, numericality: { message: "价格必须为数字" },
    if: proc { |product| !product.price.blank? }
  validates :price, presence: { message: "价格不能为空" }
  validates :description, presence: { message: "描述不能为空" }

  belongs_to :category

  before_create :set_default_attrs #产品创建之前生成唯一uuid

  module Status
    On = 'on'
    Off = 'off'
  end

  private

    def set_default_attrs
      self.uuid = RandomCode.generate_product_uuid
    end

end
```

### Step 3:修改new页面，将new和edit页面合并

打开app/views/admin/products/new.html.erb,将全部代码修改为如下内容：
```ruby app/views/admin/products/new.html.erb
<div>
  <h1><%= @product.new_record? ? "新建商品" : "修改商品 ##{params[:id]}" %></h1>
</div>

<div class="form-body">
  <%= form_for @product,
    url: (@product.new_record? ? admin_products_path : admin_product_path(@product)),
    method: (@product.new_record? ? 'post' : 'put'),
    html: { class: 'form-horizontal' } do |f| %>

    <% unless @product.errors.blank? %>
      <div class="alert alert-danger">
        <ul class="list-unstyled">
          <% @product.errors.messages.values.flatten.each do |error| %>
            <li><i class="fa fa-exclamation-circle"></i> <%= error %></li>
          <% end -%>
        </ul>
      </div>
    <% end -%>


    <div class="form-group">
      <label for="title" class="col-sm-2 control-label">名称:*</label>
      <div class="col-sm-5">
        <%= f.text_field :title, class: "form-control" %>
      </div>
    </div>
    <div class="form-group">
      <label for="category_id" class="col-sm-2 control-label">所属分类:</label>
      <div class="col-sm-5">
        <select name="product[category_id]">
          <option></option>
          <% @root_categories.each do |category| %>
            <optgroup label="<%= category.title %>"></optgroup>

            <% category.children.each do |sub_category| %>
              <option value="<%= sub_category.id %>" <%= @product.category_id == sub_category.id ? 'selected' : '' %>><%= sub_category.title %></option>
            <% end -%>
          <% end -%>
        </select>
      </div>
    </div>
    <div class="form-group">
      <label for="title" class="col-sm-2 control-label">上下架状态:*</label>
      <div class="col-sm-5">
        <select name="product[status]">
          <% [[Product::Status::On, '上架'], [Product::Status::Off, '下架']].each do |row| %>
            <option value="<%= row.first %>" <%= 'selected' if @product.status == row.first %>><%= row.last %></option>
          <% end -%>
        </select>
      </div>
    </div>
    <div class="form-group">
      <label for="quantity" class="col-sm-2 control-label">库存*:</label>
      <div class="col-sm-5">
        <%= f.text_field :quantity, class: "form-control" %> 必须为整数
      </div>
    </div>
    <div class="form-group">
      <label for="price" class="col-sm-2 control-label">价格*:</label>
      <div class="col-sm-5">
        <%= f.text_field :price, class: "form-control" %>
      </div>
    </div>
    <div class="form-group">
      <label for="msrp" class="col-sm-2 control-label">原价*:</label>
      <div class="col-sm-5">
        <%= f.text_field :msrp, class: "form-control" %>
      </div>
    </div>
    <div class="form-group">
      <label for="description" class="col-sm-2 control-label">描述*:</label>
      <div class="col-sm-5">
        <%= f.text_area :description, class: "form-control" %>
      </div>
    </div>
    <div class="form-group">
      <div class="col-sm-offset-2 col-sm-8">
        <%= f.submit (@product.new_record? ? "新建商品" : "编辑商品"), class: "btn btn-default" %>
      </div>
    </div>
  <% end -%>
</div>
```


### Step 4:删除app/views/admin/products/edit.html.erb页面
手动删除app/views/admin/products/edit.html.erb页面。

### Step 5:修改app/views/admin/products/index.html.erb

打开app/views/admin/products/index.html.erb,将全部代码修改为如下内容：
```ruby app/views/admin/products/index.html.erb
<div>
  <div class="pull-right">
    <%= link_to "新建商品", new_admin_product_path, class: "btn btn-primary" %>
  </div>

  <h1>
    商品(<%= @products.total_entries %>)
  </h1>
</div>

<div>
  <table class="table table-striped">
    <tr>
      <th>ID</th>
      <th>名称</th>
      <th>产品序列号</th>
      <th>原价</th>
      <th>现价</th>
      <th>库存</th>
      <th>上架状态</th>
      <th></th>
    </tr>

    <% @products.each do |product| %>
      <tr>
        <td><%= product.id %></td>
        <td><%= product.title %></td>
        <td><%= product.uuid %></td>
        <td><%= product.msrp %></td>
        <td><%= product.price %></td>
        <td><%= product.quantity %></td>
        <td><%= product.status %></td>
        <td align="right">
          <%= link_to "编辑", edit_admin_product_path(product) %>
          <%= link_to "删除", admin_product_path(product), method: :delete, 'data-confirm': "确认删除吗?" %>
        </td>
      </tr>
    <% end -%>
  </table>

  <%= will_paginate @products %>
</div>
```

### Step 6:将变更 commit 进去 git 里面。

```
git add .
git commit -m "优化商品管理：增加部分属性,合并new和edit页面"
git push origin story12
git checkout master
git merge story12
git push origin master
```

## 本章详解
>  努力更新中，上班族请理解。。。。。
