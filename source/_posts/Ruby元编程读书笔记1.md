layout: pages
title: Ruby元编程读书笔记1
date: 2015-12-14 17:03:04
tags: [ruby]
categories: [ruby元编程读书笔记]

---

## 第一章 元这个字眼
### 定义
 元编程就是编写能写代码的代码。
 
 Ruby 具有*自省*（introspection）。
 > 类和对象是Ruby 世界的一等公民，可以通过询问它有关自身问题。
 
 ### Active Record 类库
 
 > 可以将对象映射到数据库表中。
  
## 第二章 类的真相

### 猴子补丁

>  为某个类添加新功能

### 实例变量
通过 Object＃instance_variables方法查看

```ruby
class MyClass
	def my_method
		@v =q
	end
end
```

>ruby 实例变量的名字和值理解为 哈希表中的键和值，每一个对象的键／值都可能不相同。

### 方法
通过 Object#mothods方法查看
> 可以通过Array#grep 方法来筛选

### 类的真相
类本身也是对象。

* Ruby允许你在运行时修改类的信息。
* Ruby的类继承自他的父类。（superclass方法）
* BasicObject是Ruby对象体系中的根节点。

### Modlule
 Class的父类是Module。
 类是带有三个方法（new、allocate、superclass）的增强模块。
 

