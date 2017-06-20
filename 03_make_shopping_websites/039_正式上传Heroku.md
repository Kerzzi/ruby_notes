# 「从零开始做购物网站」第39章 正式上传Heroku

> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。

## 目标
*  将网站正式上传Heroku
*  如果你已经做了该章节部分内容，请自行跳过。

## 步骤
### Step 0:
执行```git checkout -b story31```。

### Step 1: 修改 Gemfile 档案
把 sqlite3 从第7行搬到 ```group :development, :test do ```中间
![](https://ww3.sinaimg.cn/large/006tNc79gy1fgrxlrlwq4j30dc03y747.jpg)
在末尾新增一个 production group，加上 pg 这个 gem

```
group :production do
  gem 'pg'
end
```
![](https://ww1.sinaimg.cn/large/006tNc79gy1fgrxm2nzg3j30ft03z3yg.jpg)
```command+s``` 存档

修改 Gemfile 后要 ```bundle install```。

```
git add .
git commit -m "move sqlite3 to dev group & add pg to production group"
```

### Step 2: 创建heroku app并将设定好的机密资讯同步到这个app
```heroku create``` 先创建一个heroku app (**如果已经创建过，请自行跳过**)

```figaro heroku:set -e production```

```heroku config``` 可以列出目前所有的设定

### Step 3: 上传到github和heroku

```
git push origin story31
git checkout master
git merge story31
git push origin master
git push heroku
heroku pg:reset DATABASE
heroku run rails db:migrate
heroku run rails db:seed
```

### Step 4: 开始上线商品图片吧
目前为止，购物网站的开发基本完成了。可以开始上线商品。
[该系列文章最新版地址](https://github.com/Kerzzi/ruby_notes/tree/master/03_make_shopping_websites)：https://github.com/Kerzzi/ruby_notes/tree/master/03_make_shopping_websites
网站最终效果：https://jfshop.herokuapp.com/
网站model之间的关系：
![](https://ww1.sinaimg.cn/large/006tNc79gy1fgry3vwkzkj31kw115n19.jpg)

## 本章详解
>  努力更新中，上班族请理解。。。。。
