---
title: Rails Migration 中的数据类型
categories: 
  - Blog
tags:
  - rails
  - migration
  - datatype
---

ActiveRecord 是 Rails 重要的组件之一，也是 Rails 的重要特色。它抽象化了数据的
管理，使用户可以不懂 SQL 而操作数据库。Migration 就是 Rails 驱动数据库操作数
据库的重要 ActiveRecord 工具。

## 常见数据类型

Rails 使用 Migration 来增改删数据库表。在 Migration 中，有如下数据类型：

:string

: 此数据类型用于存储文字，默认情况下，该值有最多 255 个字符的限制。但是可以
通过关键字 `limit` 在创建字段时进行限制。例如要创建一个限于 50 个字符的字段
可以使用 `t.string :name, limit: 50` 实现。

:text

: 此数据类型类似于 `:string`，但可以存储更多的内容（ 最多可达 65,535 个字符），
如同字符串也可以使用 `limit` 来限制字符的长度。

:integer

: 此数据类型用于存储整数（因为可能有歧义，可以理解为非小数）。默认整数值为
-2147483648 ～ 2147483647 区间的任意值都可以。如果需要限定偏大或者偏小的值，
可以通过 `limit` 参数来限制取值范围。例如，为了创建介于 -32768 ～ 32767 之间
的整数，可使用下面语法：`t.integer :age, limit: 2`。`limit` 取值范围如下：

|limit | PostgreSQL | MySQL | Size(bytes) | Max |
|:-----|:----------:|:-----:|:-----------:|----:|
| 1 | | tinyint | 1 | 127 |
| 2 | smallint/int2 | smallint | 2 | 32767 |
| 3 | | mediumint | 3 | 8388607 |
| 4 (default) | integer/int/int4 | int | 4 | 2147483647 |
| 5 to 8 | bigint/int8 | bigint | 8 | 9223372036854775807 |

:float

: 此数据类型用于存储小数，默认使用 8 位内存存储，可以通过使用高精度存储更多
位数的小数。

:decimal

: 此数据类型雷同于 `:float`，但是可以通过精度（制定小数点后面的位数）设定小
数值。这可以用来设定高精度的金融值。

:datetime

: 此数据类型用于存储日期和时间值。它存储日期和时间作为单一的时间戳，可用于
比较和处理。

:timestamp

: 此数据类型雷同于 `:datatime`，但它存储的是数据创建和更新时的数据。这对于数据
追踪很有用

:boolean

: 此数据类型用于存储 `true/false` 值，常用于代表某些状态（例如用户是否是活
跃的等。）

## Migration 数据类型与各种数据库类型对应表

常见数据库对应关系如下：

| 数据类型 | PostgreSQL | sqlite3 | MySQL | Oracle | SQLSever |
|:---------|:----------:|:-------:|:-----:|:------:|---------:|
| :binary | bytea | blob | blob | blob | image |
| :boolean | boolean | boolean | tinyint(1) | number(1) | bit |
| :date | date | date | date | date | date |
| :datetime | timestamp | datetime | datetime | datetime | datetime |
| :decimal | decimal | decimal | decimal | decimal | decimal |
| :float | float | float | float | number | float(8) |
| :integer | integer | integer | int(11) | number(38) | int |
| :bigint | bigint | | | | |
| :string | character varying | varchar(255) | var-char(255) | varchar2(255) | varchar(255) |
| :text | text | text | text | clob | text |
| :time | datetime | datetime | time | date | time |
| :timestamp | timestamp | datetime | datetime | date | datetime |

此外，Rails 还有一些特定的数据类型，例如存储二进制数据 `:binary`，地理信息数
据的 `:point, :line_string, :polygon`等，以及 JSON 数据的 `:json, :jsonb`等。

总之，在使用 Migration 创建表的时候，可以使用各种参数定制数据的存储和显示。
例如，可以使用 `default` 设定字段的默认值，可以设定字段的 `null` 参数来控制
字段是否允许为空等。

通过 Migration 可以定制表的很多方面，减少前端的验证，优雅编程！
