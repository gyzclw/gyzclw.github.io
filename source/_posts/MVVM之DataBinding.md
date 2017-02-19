layout: pages
title: MVVM之DataBinding
date: 2016-08-14 21:58:17
tags:
---
android DataBinding 的使用，注意：
```html
 <data>

    <variable
        name="viewModel"
        type="com.mega.viewmodel.UserViewModel"/>
  </data>

   <Button
        android:id="@+id/login"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@+id/user_password"
        android:background="@color/colorPrimaryDark"
        android:onClick="@{viewModel.setLogin}"
        android:text="Login"
        android:textColor="@color/wild_sand"
        />
```
要使用viewModel的 onclick生效必须在View中绑定ViewModel
```java
  private ActivityLoginBinding loginBinding;
  loginBinding = DataBindingUtil.setContentView(this, R.layout.activity_login);
  loginBinding.setViewModel(new UserViewModel());
```
##  viewModel 的作用
The presenter only interacts with the view interface，get data from Model,
It retrieves data from the model and returns it formatted to the view. But unlike the typical MVC, it also decides what happens when you interact with the view.
