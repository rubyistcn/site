---
title: Rails 的验证是在前端还是后端
categoires:
  - blog
tags:
  - rails
  - validates
  - database
---

Rails 应用中通常都是前端 HTML+CSS+Javascript/后端数据库的组合。
那么，在数据输入过程中，数据的规范化检查是应该在前端做还是后端做？

这里所说的前端可能还有两种情况：

1. 使用纯 HTML+Javascript 进行检查；
2. 在数据进入数据库前，利用 Rails 检查；

这两种检查中，第一种常用于一些简单验证，对于业务逻辑比较复杂的情况，可能
就需要使用第二种方法进行验证检查了。

我们知道，数据库系统本身也具有一些数据验证的功能——例如简单的数据是否需要
填写，以及基本的数据格式是否符合等，但是一旦涉及到比较复杂的业务数据逻辑，
就会显得力不从心。而且，Rails 本身的设计，其实也是在弱化不同数据库间的差异，
同时尽量减少数据库对编程人员的干扰。例如 Migration 的设计，就是在统一对数据
库的操作。虽然，对于某些数据库系统可能做出优化，例如 Postgresql，但是，总体
而言，Rails 的设计宗旨就是要消除数据库的差异，而统一操作接口，使编程更优雅
自然。

所以，在 Rails 应用的设计中，数据验证可以在数据库端或者利用 Javascript 在前
端进行，但是即使在前端和数据库中做了数据验，还是需要在 Rails 的 Model 中进行
数据的验证处理，这样做一是为了万无一失，二是在 Model 端进行的逻辑验证，可以
更好的进行信息反馈，进行人机交互，更加人性化。
