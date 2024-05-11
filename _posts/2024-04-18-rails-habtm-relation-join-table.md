---
title: Rails 中的多对多关联数据表
categories: 
  - Blog
tags:
  - rails
  - relation
  - habtm
---

多对多关联，是 Rails 中相对复杂的一种关系表。为了便于管理，往往都会创建一个
中间表，用来记录关联表之间的关系。

例如：一个作者（Author）可以写多部著作(Book)，一部著作也有可能多人合作完成。
作者与著作之间的关系就是多对多的关系，在Rails 中专门可以使用 `has_and_belongs_to_many`
表述。用于管理的连接表通常就会用两个表的名字进行连接获得，这里在我们的示例
中的连接表就应该叫做 `authors_books`。那么，叫做 `books_authors` 可以吗？理论
上是可以的，但是在 Rails 中，按照惯例，会将两个表的名字按照字符串的大小进行
比较，小的在前，大的在后，所以应该是 `authors_books`。

在实际项目中，如果项目很大，数据表众多，我会将与某一部分资源相关的表进行按
照某资源统一命名。例如我做的关于古籍（表叫做 `ancient_books`）的部分，就会
以 `ancient_book` 作为前缀进行命名。于是，关于古籍收藏方式（Store Style）的表
就命名为 `ancient_book_store_styles`。一种古籍会有多种收藏方式，一个收藏方式
当然也会用于多种古籍，这也很正常。所以需要使用多对多关系进行关联，手动创建
连接表格则应该是 `ancient_book_store_styles_ancient_books`。然后，在 Model 中
设定 `has_and_belongs_to_many` 关系，测试。结果就出现了找不到表格“ancient_book_store_styles_books”的错误，很奇怪！

后来，决定使用 Rails 的 Migration 中的 `create_join_table` 方法由 Rails 进行创
建连接表，代码如下

```ruby
def change
  create_join_table :ancient_book, :ancient_book_store_styles
end
```

系统自动创建的表叫做 `ancient_book_store_styles_books`，而不是我们以为的 `ancient_book_store_styles_ancient_books`，这个由系统自动生成的表 Rails 当然会
找到了。

关于更多 `create_join_table` 说明参见[这里](https://apidock.com/rails/ActiveRecord/ConnectionAdapters/SchemaStatements/create_join_table)
