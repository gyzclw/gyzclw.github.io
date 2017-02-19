layout: pages
title: Android 使用 LruCache 缓存图片
date: 2015-12-10 11:27:32
categories: [android]
tags: [android]
 
---

## Android 使用 LruCache 缓存图片


在你应用程序的 UI 界面加载一张图片是一件很简单的事情，但是当你需要在界面上加载一大堆图片的时候，情况就变得复杂起来。在很多情况下，（比如使用 ListView, GridView 或者 ViewPager 这样的组件），屏幕上显示的图片可以通过滑动屏幕等事件不断地增加，最终导致 OOM。为了保证内存的使用始终维持在一个合理的范围，通常会把被移除屏幕的图片进行回收处理。此时垃圾回收器也会认为你不再持有这些图片的引用，从而对这些图片进行 GC 操作。用这种思路来解决问题是非常好的，可是为了能让程序快速运行，在界面上迅速地加载图片，你又必须要考虑到某些图片被回收之后，用户又将它重新滑入屏幕这种情况。这时重新去加载一遍刚刚加载过的图片无疑是性能的瓶颈，你需要想办法去避免这个情况的发生。

这个时候，使用内存缓存技术可以很好的解决这个问题，它可以让组件快速地重新加载和处理图片。下面我们就来看一看如何使用内存缓存技术来对图片进行缓存，从而让你的应用程序在加载很多图片的时候可以提高响应速度和流畅性。内存缓存技术对那些大量占用应用程序宝贵内存的图片提供了快速访问的方法。其中最核心的类是 LruCache ( 此类在 android-support-v4 的包中提供 ) 。这个类非常适合用来缓存图片，它的主要算法原理是把最近使用的对象用强引用存储在 LinkedHashMap 中，并且把最近最少使用的对象在缓存值达到预设定值之前从内存中移除。

<!-- more -->

在过去，我们经常会使用一种非常流行的内存缓存技术的实现，即软引用或弱引用 (SoftReference or WeakReference)。但是现在已经不再推荐使用这种方式了，因为从 Android 2.3 (API Level 9)开始，垃圾回收器会更倾向于回收持有软引用或弱引用的对象，这让软引用和弱引用变得不再可靠。另外，Android 3.0 (API Level 11)中，图片的数据会存储在本地的内存当中，因而无法用一种可预见的方式将其释放，这就有潜在的风险造成应用程序的内存溢出并崩溃。为了能够选择一个合适的缓存大小给 LruCache, 有以下多个因素应该放入考虑范围内，例如：

1. 你的设备可以为每个应用程序分配多大的内存？
2. 设备屏幕上一次最多能显示多少张图片？
3. 有多少图片需要进行预加载，因为有可能很快也会显示在屏幕上？
4. 你的设备的屏幕大小和分辨率分别是多少？一个超高分辨率的设备（例如 Galaxy Nexus）比起一个较低分辨率的设备（例如 Nexus S），在持有相同数量图片的时候，需要更大的缓存空间。
5. 图片的尺寸和大小，还有每张图片会占据多少内存空间？
6. 图片被访问的频率有多高？会不会有一些图片的访问频率比其它图片要高？如果有的话，你也许应该让一些图片常驻在内存当中，或者使用多个 LruCache 对象来区分不同组的图片。
7. 你能维持好数量和质量之间的平衡吗？有些时候，存储多个低像素的图片，而在后台去开线程加载高像素的图片会更加的有效。

并没有一个指定的缓存大小可以满足所有的应用程序，这是由你决定的。你应该去分析程序内存的使用情况，然后制定出一个合适的解决方案。一个太小的缓存空间，有可能造成图片频繁地被释放和重新加载，这并没有好处。而一个太大的缓存空间，则有可能还是会引起 java.lang.OutOfMemory 的异常。

下面是一个使用 LruCache 来缓存图片的例子：

```java
private LruCache<String, Bitmap> mMemoryCache;

@Override
protected void onCreate(Bundle savedInstanceState) {
    // 获取到可用内存的最大值，使用内存超出这个值会引起OutOfMemory异常。
    // LruCache通过构造函数传入缓存值，以KB为单位。
    int maxMemory = (int) (Runtime.getRuntime().maxMemory() / 1024);
    // 使用最大可用内存值的1/8作为缓存的大小。
    int cacheSize = maxMemory / 8;
    mMemoryCache = new LruCache<String, Bitmap>(cacheSize) {
        protected int sizeOf(String key, Bitmap bitmap) {
            // 重写此方法来衡量每张图片的大小，默认返回图片数量。
            return bitmap.getByteCount() / 1024;
        }
    };
}

public void addBitmapToMemoryCache(String key, Bitmap bitmap) {
    if (getBitmapFromMemCache(key) == null) {
        mMemoryCache.put(key, bitmap);
    }
}

public Bitmap getBitmapFromMemCache(String key) {
    return mMemoryCache.get(key);
}
```
在这个例子当中，使用了系统分配给应用程序的八分之一内存来作为缓存大小。在中高配置的手机当中，这大概会有4兆(32/8)的缓存空间。一个全屏幕的 GridView 使用 4 张 800x480 分辨率的图片来填充，则大概会占用 1.5 兆的空间 (800*480*4)。因此，这个缓存大小可以存储 2.5 页的图片。当向 ImageView 中加载一张图片时,首先会在 LruCache 的缓存中进行检查。如果找到了相应的键值，则会立刻更新 ImageView ，否则开启一个后台线程来加载这张图片。

```java
public void loadBitmap(int resId, ImageView imageView) { 
    final String imageKey = String.valueOf(resId); 
    final Bitmap bitmap = getBitmapFromMemCache(imageKey); 
    if (bitmap != null) { 
        imageView.setImageBitmap(bitmap); 
    } else { 
        imageView.setImageResource(R.drawable.image_placeholder); 
        BitmapWorkerTask task = new BitmapWorkerTask(imageView); 
        task.execute(resId); 
    } 
}
BitmapWorkerTask 还要把新加载的图片的键值对放到缓存中。

class BitmapWorkerTask extends AsyncTask<Integer, Void, Bitmap> { 
    // 在后台加载图片。 protected Bitmap doInBackground(Integer... params) { 
        final Bitmap bitmap = decodeSampledBitmapFromResource( 
                getResources(), params[0], 100, 100); 
        addBitmapToMemoryCache(String.valueOf(params[0]), bitmap); 
        return bitmap; 
    } 
}
```