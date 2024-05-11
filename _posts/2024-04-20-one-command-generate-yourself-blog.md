---
title: 一个命令生成自己的网志
categories:
  - blog
tags:
  - rails
  - blog
---

Rails 是以 Gem 形式发布的，所以在安装好 Ruby 以后，就已经安装了 Gem 管理系统。再安装
Rails 就比较简单了。

```shell
gem install rails
```

安装完 Rails，或者已经安装过 Rails，就可以生成一个 Rails 应用程序：

```ruby
rails new blog
```

“blog”就是我们的应用程序名字。

进入的 `blog` 目录，生成我们的应用程序实际内容：

```ruby
rails generate scaffold post title:string content:text
```
然后启动服务器：

```ruby
rails server
```
在浏览器地址栏中输入 `http://localhost:3000/posts` 就可以使用我们的应用程序了。
