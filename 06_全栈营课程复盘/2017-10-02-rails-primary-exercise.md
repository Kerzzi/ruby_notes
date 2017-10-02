---
layout:     post                       # 使用的布局（不需要改）
title:      课程复盘 | Rails 第二课：初级练习                 # 标题
subtitle:   新生大学全栈营 课程复盘 #副标题
date:       2017-10-02                 # 时间
author:     Kerzzi                         # 作者
header-img: img/zhihu.jpg     #这篇文章标题背景图片
categories: Ruby on Rails                 # 分类
tags:                                #标签
    - 新生大学全栈营
    - 全栈营课程复盘
    - Ruby on Rails
---

## 课程目标
> 为了让你用最快的速度上手 Ruby on Rails，我们会使用“真实世界”的范例。
> 现在我们要实际做出一个投票系统， 这是一个基本可用的程式，让使用者可以：

> * 检视主题，用投票数来排序
> * 对主题投票
> * 开新主题、编辑主题、删除主题
> * 贴张图让你看看它会长怎样：

![](https://ww3.sinaimg.cn/large/006tKfTcgy1fk3pumnk8nj30be07vjrl.jpg)

## 复盘

### 1. 一些常用指令：

```
rm -rf suggestotron   # 删除文件夹指令
cd suggestotron    # cd指的是切换目录（change directory）
ls   # 指的是列出东西 (list stuff)。
```

### 2. 把项目加入Git Repo

```
pwd  #确认你在目标目录里
git init
git status
git add .
git commit -m "Added all the things"
```

### 3. 在终端机打这些字：

```
rails generate scaffold topic title:string description:text
```

![](https://ww3.sinaimg.cn/large/006tKfTcgy1fk3ptol9o6j30p9071t9d.jpg)

图片中： **invoke 意思为“调用”**。

### 4. CRUD

> C - 建立（Create）新资料并**存进资料库**。
> R - 读取（Read）或显示资料库里的资料。
> U - **更新**（Update）既有的资料,并**存进资料库**。
> D - 删除（**Destroy**）资料。

### 5. 部署到Heroku上

> 第一步：在终端（Terminal）里输入```heroku create```
> 第二步：打开 Gemfile 文件, **迁移sqlite3**
> ![](https://ww1.sinaimg.cn/large/006tKfTcgy1fk3q97m3acj30p70pntab.jpg)
> 变成下图这样
> ![](https://ww4.sinaimg.cn/large/006tKfTcgy1fk3q9b2aisj30oi0kl3zl.jpg)
> 加'pg'这个gem
> ![](https://ww3.sinaimg.cn/large/006tKfTcgy1fk3q9vl80bj30o80dqaak.jpg)
> 存档，运行```bundle install```

> 第三步：保存修改

> ```
> git add .
> git commit -m "move sqlite3 to dev group & add pg to production group"
> ```

> 第四步：最后再输入 ```git push heroku master```
> 第五步：部署成功后，在终端（Terminal）输入 ```heroku run rake db:migrate```
> 第六步：最后在终端输入 ```heroku open``` 就可以打开刚刚部署到heroku上的app了。

### 6. 新建model文件

```
rails generate model vote topic_id:integer
rake db:migrate
```

### 7. 告诉 Topic model 有 Vote（投票记录）的存在

```ruby app/models/topic.rb
class Topic < ApplicationRecord
  has_many :votes, dependent: :destroy
end
```

### 8. 进 Rails console 随便玩玩看 Topic 和 Vote model

```
rails c
Topic.count
my_topic = Topic.first
my_topic.update_attributes(title: 'Edited in the console') # 常用功能
my_topic.votes.create
my_topic.votes.count
my_topic.votes.first.destroy #删除一笔那个 topic 的投票记录

```

最后，记得输入exit退出console。

请注意，你在 **Model class**（例如 **Topic、Vote**）和 **Model instance**（例如此例的my_topic）可以做的事是不一样的。my_topic.votes称作 association，在这里的行为比较像是 model class。
![](https://ww3.sinaimg.cn/large/006tKfTcly1fk3qqs9v1lj30m8072gm0.jpg)

全部方法英文版：[Active Record Query Interface](http://guides.rubyonrails.org/active_record_querying.html)
全部方法中文版：[Active Record 查詢](https://rails.ruby.tw/active_record_querying.html)

### 9. 增加新的controller之后，需要加入相应的route和view

* 加一个新的 controller action 来投票加分

```ruby app/controllers/topics_controller.rb
def upvote
  @topic = Topic.find(params[:id]) #从资料库里面用 id 找到 topic 然后存进 @topic 变数里面
  @topic.votes.create #给目前这篇 topic 新增一笔投票记录，然后存进资料库里面
  redirect_to(topics_path)  #跟浏览器说要回到 topics_path（topics 的列表）
end
```

* 给投票加分操作加一个 route

```ruby config/routes.rb
Rails.application.routes.draw do
 root 'topics#index'
 resources :topics do
   member do
     post 'upvote'
   end
 end
end
```

检查 route 有成功加入，方法是看一下 ```rake routes``` 的输出结果。

* 在 view 里面加一个按钮

```ruby app/views/topics/index.html.erb
<% @topics.each do |topic| %>
  <tr>
    <td><%= topic.title %></td>
    <td><%= topic.description %></td>
    <td><%= pluralize(topic.votes.count, "vote") %></td>  
    <td><%= button_to '+1', upvote_topic_path(topic), method: :post %></td>
    <td><%= link_to 'Show', topic %></td>
    <td><%= link_to 'Edit', edit_topic_path(topic) %></td>
    <td><%= link_to 'Destroy', topic, method: :delete, data: { confirm: 'Are you sure?' } %></td>
  </tr>
<% end %>
```

代码解释：
```pluralize(topic.votes.count, "vote")```
会输出票数，并且根据（英文的）单复数在后面加上 'vote' 或 'votes' 单字，pluralize：以复数形式表示。

```button_to '+1' ```
加一个 HTML 的按钮（button），里面有字 '+1'。

```upvote_topic_path(topic)```
产生我们要呼叫的 action 的对应 URL。以此例而言，我们要对目前的 topic 投票加分。它会回传/topics/42/upvote（如 topic.id 是 42）

```method: :post```
确保我们使用了 CRUD 里面的 create 操作，而非 read（method: :get）。

### 10. 把标题变成超连结

做如下修改：

```ruby app/views/topics/index.html.erb
- <td><%= topic.title %></td>
+ <td><%= link_to topic.title, topic %></td>
```

代码中 topic.title 是链接显示的内容(这里为标题)，也可以直接用双引号扩住显示的内容。topic是该动作要去的url。类别下这一句```<td><%= link_to 'Show', topic %></td>```

### 11. Rails 的架构以及它与资料库的关联

Rails 把 Model/View/Controller 的设计模式实作得非常具体，来指引你如何设计你的网路应用程式。

**Model**

* 我们在 RailsBridge 建立的 Model，其每一个 Model object 都在资料库里面有对应的资料。**资料库里面的表格（table）是 Model 的 class name 的复数形**。例如，如果 Model 叫做 'Duck'，就会自动去资料库读写 'ducks' 这个表格。
* Rails 内部的 methods 让我们可以很容易把资料写入到资料库，之后再从资料库里面查询（query）出来。
* Model 是介于资料库和你的程式码之间的桥梁。


**View**

* View 会产生 HTML 来显示在浏览器。
* View 档案是用 ERB 写的，它是一种样板语言（Template Language）。里面是 HTML 加上内嵌的 Ruby 程式码。View 里面的 Ruby 的变数便是当使用者要浏览该页面的时候，所要填入的内容。
* （还有别的样板语言，但是在 RailsBridge 我们只用 ERB。）


**Controller**

* Controller 把 Ruby 的 objects 在 Model 和 View 之间传来传去。
* 每一个 URL 都对应到 Controller 里面的某一个特定的 method。
* 在这一步骤以后，当你打开你应用程式里面的任何一个页面，该请求（request）会被某个 Controller 的 method 处理。

当我们把 Models、Views、Controllers 放在一起的时候，他们会遵循以下的模式：

给一个 URL，Rails 会去检查要使用哪一个 Controller 里面的 method（又称为 "Action"）。Controller Action 会去呼叫 Model 里面对应的 methods。Model 会去读写资料库，然后把包含资料的 object 回传到 Controller。Controller 会拿到这个 object 并且丢到 View 里面。Action 通常会有对应的 View 档案，Rails 会自动寻找并使用之。）

Models、Views、Controllers 有各自的工作。像这样把责任拆开来，会比较容易开发，尤其是它长得愈来愈大的时候。（每个档案都有清晰的责任，会比较容易处理问题、增加新功能。）
