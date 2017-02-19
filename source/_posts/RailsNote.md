layout: pages
title: RailsNote
date: 2015-12-11 13:10:33
categories: [ruby]
tags: [ruby]

---
# 知识点

## befor_action 方法
>Append a callback before actions. See #_insert_callbacks for parameter details.


在events_controller.rb上方新增

```ruby
before_action :method_name, :only => [ :show, :edit, :update, :destroy]
```

在 private下方

```ruby
private
def method_name
	<-内容->
end
```
## use pry to rails console

```
gem 'pry-rails', :group => :development
```
## flash vs. flash.now

* flash 方法设置flash message , 显示数据在指定页面，需要redirect_to 指定页面
* flash_now 可替代flash ,在当前页面显示flash message 
## ActiveModel::Errors 方法
ActiveModel::Errors provides some nice helper methods for working with validation errors that you can use in your views to display the errors to the user.

## partials 的命名
partials的命名必须使用_开头，例如_form而不是form