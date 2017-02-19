layout: pages
title: RubyNotes
date: 2016-01-07 10:43:08

tags: [Ruby]

---

# Ruby 备忘录

> gem源切换：nrm use

##:: 

> ActiveRecord::Base Base 是 ActiveRecord module 的一个内部类 

## Ruby Varable

一般小写字母、下划线开头：变量（Variable）。
* $开头：全局变量（Global variable）。
* @开头：实例变量（Instance variable）。
* @@开头：类变量（Class variable）类变量被共享在整个继承链中
* 大写字母开头：常数（Constant）。

## Custome Objects in Range

>要构造一个Range 对象需要 include Comparable 模块，异常处理：实现一个succ方法返回类的新的对象;还要实现一个<=>方法  

```ruby
class Xs                # represent a string of 'x's
  include Comparable
  attr :length
  def initialize(n)
    @length = n
  end
  def succ
    Xs.new(@length + 1)
  end
  def <=>(other)
    @length <=> other.length
  end
  def to_s
    sprinf "%2d #{inspect}",@length
  end
  def inspect
    'x' *@length
  end
end
```
## Manning Rails
### Database configuration
rails 默认使用SQlite3 作为数据库，当然也支持MySQL 和PostgreSQL 数据库。
如果要更改数据库，可以在conifg/database.yml 中修改
例如：使用sqlite3
```rails
development:
  adapter: sqlite3
  database: db/development.sqlite3
  pool: 5
  timeout: 5000
```
使用PostgreSQL
```rails
development:
  adapter: postgresql
  database: ticketee_development
  username: root
  password: t0ps3cr3t
```
PostgreSQL 数据库比SQlite更快

```rails
development:
  adapter: mysql2
  database: ticketee_development
  username: root
  password: t0ps3cr3t
  encoding: utf-8
```
