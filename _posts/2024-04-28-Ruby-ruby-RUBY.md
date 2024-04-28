---
title: "Ruby、ruby 还是 RUBY？"
date: 2024-04-28T06:20:02-05:00
categories:
  - Blog
tags:
  - well-grouned-rubyist
  - Ruby
---

Ruby，是红宝石的专有名词。但是在计算机领域里，Ruby 通常就是指一种动态编程语言。
写法就是 `Ruby`，第一个字母大写，其它三个字母小写。

如果全部小写的时候（`ruby`）呢？这就是 Ruby 语言的命令行模式下的调用方式，后面
可以接一个程序文件，或者参数等等，例如

```ruby
ruby helloworld.rb
```

或者

```ruby
ruby -run -e httpd .
```

如果全部大写（RUBY）呢？这就跟 Ruby 语言没啥关系了。

全部小写的 `ruby`，在 HTML 里，还是一个用来表示东亚语言注音的标签。例如

<ruby>
  汉 <rp>(</rp><rt>Han</rt><rp>)</rp>
  字 <rp>(</rp><rt>zi</rt><rp>)</rp>
</ruby>

总之，跟 Ruby 语言有关的写法就两种：

1. Ruby：语言名字
2. ruby：Ruby 语言的解释器调用
