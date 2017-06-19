# 【ORID】JDstore魔改大赛复盘
这是我参加全栈营JDstore魔改大赛的制作过程复盘，由于本人是ROR新手，文章难免错误百出，欢迎您提出改进意见，可以在我的Github上留言，或者发邮件至kerzzi@outlook.com，我会持续更新这个系列，努力的去完善它。

[网站效果演示](https://jfshop.herokuapp.com/)：https://jfshop.herokuapp.com/

> 该系列文章会不定期在我的[Github](https://github.com/Kerzzi/ruby_notes)上更新：https://github.com/Kerzzi/ruby_notes。

## 教程目录：

* [「从零开始做购物网站」第1-11章 全栈营课程 JDstore demo(付课程详细讲解)](http://kerzzi.logdown.com/categories/%E4%BB%8E%E9%9B%B6%E5%BC%80%E5%A7%8B%E5%81%9A%E8%B4%AD%E7%89%A9%E7%BD%91%E7%AB%99)
* [「从零开始做购物网站」第12章 暂停一下](http://kerzzi.logdown.com/posts/1951227-starting-from-scratch-do-shopping-site-pause-the-12th-chapter)
* [「从零开始做购物网站」第13章 增加弹窗登录登出效果](http://kerzzi.logdown.com/posts/1951255-starting-from-scratch-do-shopping-site-the-13th-chapter-to-increase-pop-login-logout-effect)
* [「从零开始做购物网站」第14章 建立服务、关于等静态页面](http://kerzzi.logdown.com/posts/1951262)
* [「从零开始做购物网站」第15章 实作商品多级分类功能(1)](http://kerzzi.logdown.com/posts/1951278)
* [「从零开始做购物网站」第16章 实现前后台分离](http://kerzzi.logdown.com/posts/1951280)
* [「从零开始做购物网站」第17章 完成商品分类功能开发（2）](http://kerzzi.logdown.com/posts/1951281)
* [「从零开始做购物网站」第18章 优化商品管理：增加部分属性,合并new和edit页面](http://kerzzi.logdown.com/posts/1951282-starting-from-scratch-do-shopping-site-the-18th-chapter-of-optimizing-product-management-add-several-properties-merge-and-edit-new-page)
* [「从零开始做购物网站」第19章 暂停使用carrierwave，选用paperclip进行多图上传](http://kerzzi.logdown.com/posts/1951283-starting-from-scratch-do-shopping-site-the-19th-chapter-suspended-carrierwave-use-paperclip-multiple-upload)
* [「从零开始做购物网站」第20章 分类和商品展示页面开发](http://kerzzi.logdown.com/posts/1951284)
* [「从零开始做购物网站」第21章 增加商品点赞功能](http://kerzzi.logdown.com/posts/1951285-starting-from-scratch-do-shopping-site-21st-chapter-to-increase-commodity-like-functionality)
* [「从零开始做购物网站」第22章 增加商品评论功能](http://kerzzi.logdown.com/posts/1951287)
* [「从零开始做购物网站」第23章 开发个人中心和默认收货地址](http://kerzzi.logdown.com/posts/1951289)
* [「从零开始做购物网站」第24章 使用SendCloud服务发送邮件](http://kerzzi.logdown.com/posts/1951291)
* [「从零开始做购物网站」第25章 利用paperclip和qiniuyun在Heroku上显示图片](http://kerzzi.logdown.com/posts/1951292)
* [「从零开始做购物网站」第26章 实作商品社交分享功能](http://kerzzi.logdown.com/posts/1951301)
* [「从零开始做购物网站」第27章 增加客服功能](http://kerzzi.logdown.com/posts/1951302)
* [「从零开始做购物网站」第28章 给购物车增加加减按钮](http://kerzzi.logdown.com/posts/1951303-starting-from-scratch-do-shopping-site-the-28th-chapter-added-to-the-shopping-cart-plus-and-minus-buttons)
* [「从零开始做购物网站」第29章 美化购物车界面](http://kerzzi.logdown.com/posts/1951304)
* [「从零开始做购物网站」第30章 美化商品展示页面，增加收藏商品功能](http://kerzzi.logdown.com/posts/1951306-starting-from-scratch-do-shopping-site-the-30th-chapter-landscaping-product-display-page-increase-their-stock-of-product-features)
* [「从零开始做购物网站」第31章 新增商品多级搜索功能](http://kerzzi.logdown.com/posts/1951308)
* [「从零开始做购物网站」第32章 美化服务页面](http://kerzzi.logdown.com/posts/1951310-starting-from-scratch-do-shopping-sites-chapter-32nd-landscaping-services-page)
* [「从零开始做购物网站」第33章 美化navbar、footer、商品首页](http://kerzzi.logdown.com/posts/1951311)
* [「从零开始做购物网站」第34章 实作landingpage页面](http://kerzzi.logdown.com/posts/1951313)
* [「从零开始做购物网站」第35章 美化关于页面](http://kerzzi.logdown.com/posts/1954882-starting-from-scratch-do-shopping-site-the-35th-chapter-implement-paypal-payment)
* [「从零开始做购物网站」第36章 实作支付宝真实支付功能](http://kerzzi.logdown.com/posts/1954884-starting-from-scratch-do-shopping-site-the-36th-chapter-implement-paypal-payment)
* [「从零开始做购物网站」第37章 实作微信真实支付功能](http://kerzzi.logdown.com/posts/1954886-starting-from-scratch-do-shopping-site-the-37th-chapter-implementing-micro-payment-function)
* [「从零开始做购物网站」第38章 大扫除：清理一系列小BUG](http://kerzzi.logdown.com/posts/1954888)
* [「从零开始做购物网站」第39章 正式上传Heroku](http://kerzzi.logdown.com/posts/1954892)
* [「从零开始做购物网站」第40章 超强彩蛋来袭！](http://kerzzi.logdown.com/posts/1954893)
