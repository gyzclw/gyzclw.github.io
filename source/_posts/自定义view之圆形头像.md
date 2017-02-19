layout: pages
title: 自定义view之圆形头像
date: 2016-08-13 22:47:03
tags: [android]
---
# 实现的思路
	在开发中需要实现头像功能，本来是直接从网上找到开源图片库，不过我觉得需要提升自己的。所以开始自定义view 的功能，
	先分析一下实现的思路，android 自带的ImageView 是显示图片是长方形，而我们需要实现的是圆形的展示图片，所以实际上就是将长方形变成圆形，而圆形显示需要先求出圆的半径r。
新建一个长方形，然后大小设置为自定义View的大小，圆的半径就等于宽和高中较短的
一半。
```java
        mBorderPaint.setStrokeWidth(mBorderWidth);
        mBitmapWidth = mBitmap.getWidth();
        mBitmapHeight = mBitmap.getHeight();
        mBorderRect.set(0, 0, getWidth(), getHeight());
        mBorderRadius = Math.min((mBorderRect.width() - mBorderWidth) / 2,
            (mBorderRect.height() - mBorderWidth) / 2);
        mDrawableRect.set(0, 0, mBorderRect.width() - mBitmapWidth,
            mBorderRect.height() - mBitmapWidth);
        mDrawableRadius = Math.min(mDrawableRect.width() / 2, mDrawableRect.height() / 2);
        updateShaderMatrix();
        invalidate();
    }
```
接下来获取圆点的坐标
```java
  x = getWidth()  / 2 
  y = getHeight() / 2
```
通过Paint来绘制图片。
```java
    mBitmapShader = new BitmapShader(mBitmap, Shader.TileMode.CLAMP, Shader.TileMode.CLAMP);
    private final Paint mBitmapPaint = new Paint();
    mBitmapPaint.setAntiAlias(true);
    mBitmapPaint.setShader(mBitmapShader);
```
重写onDraw()方法
```java
canvas.drawCircle(getWidth() / 2, getHeight() / 2, mDrawableRadius, mBitmapPaint);
```
