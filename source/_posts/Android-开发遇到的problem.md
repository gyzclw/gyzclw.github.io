title: Android 开发遇到的problem
date: 2015-12-09 15:11:34
tags:
 - android
categories:
 - android

---

> 1、Error:(23, 13) Failed to resolve: com.jakewharton:butterknife:7.0.1

![](http://7xp12c.com1.z0.glb.clouddn.com/static/images/2015120132.png)

![](http://7xp12c.com1.z0.glb.clouddn.com/static/images/2015120134.png)

## android use retrolambda

```groovy
apply plugin: 'me.tatarka.retrolambda'

buildscript {
//开始加入
 dependencies {
  classpath 'me.tatarka:gradle-retrolambda:3.1.0'
   }
    //结束插入
    }

compileOptions {
      sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
	        }

```
android 适配技巧
在设置不同屏幕比例的values文件来适配不同屏幕分辨率的手机。在dimen中设置文件大小
