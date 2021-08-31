@[TOC]



<br>
<br>
<br>
<br>

# 一、Flutter 启动白屏问题

---

<br>


启动 Flutter 应用 , 在 Launcher 主界面中 , 点击 Flutter 应用图标 , 之后出现白屏 1 ~ 5 秒 , 才能显示 Flutter 界面 ;

手机性能越高 , 白屏时间越短 ;


![请添加图片描述](https://img-blog.csdnimg.cn/ac4ff51ad1814b3ba036dc23d9b2da12.gif)

上述启动白屏问题 , 是 Flutter 框架自身的问题 , 不论是 Android 还是 iOS , 都会有上述问题 ;

<br>

Flutter 应用启动时 , 会先初始化 Flutter SDK , 然后将 Flutter 代码和资源加载到内存中 , 在内存中进行图像渲染 ;

从 Flutter 启动 , 到 渲染完毕 , 这个过程之间 , 没有任何内容显示 , 因此会出现白屏 ;

<br>

解决上述问题 , 与 Android 启动优化类似 , 给其加载一个默认背景界面 , 让 Flutter 应用在白屏的这几秒 , 显示一个图片 ;


<br>

直接参考 Android 启动优化方案 [【Android 性能优化】应用启动优化 ( 主题背景图片设置 | 设置透明主题背景 | 设置应用启动主题背景、启动后恢复主题 )](https://hanshuliang.blog.csdn.net/article/details/106870131) ;






<br>
<br>
<br>
<br>

# 二、在 launch_background.xml 中设置启动过渡 UI

---

<br>



目前 Flutter 解决上述问题 , 已经比较完善 , 不需要做过多的设置 ;

打开 Flutter 工程下的 Android 工程的  , 可以看到如下注释 :

```xml
            <!-- Specifies an Android theme to apply to this Activity as soon as
                 the Android process has started. This theme is visible to the user
                 while the Flutter UI initializes. After that, this theme continues
                 to determine the Window background behind the Flutter UI. -->
            <meta-data
              android:name="io.flutter.embedding.android.NormalTheme"
              android:resource="@style/NormalTheme"
              />
            <!-- Displays an Android View that continues showing the launch screen
                 Drawable until Flutter paints its first frame, then this splash
                 screen fades out. A splash screen is useful to avoid any visual
                 gap between the end of Android's launch screen and the painting of
                 Flutter's first frame. -->
            <meta-data
              android:name="io.flutter.embedding.android.SplashScreenDrawable"
              android:resource="@drawable/launch_background"
              />
```

配置的 io.flutter.embedding.android.SplashScreenDrawable 参数 , 就是在 Android 启动过后到 Flutter 渲染之前 , 显示的 Android 视图 , 该视图会慢慢淡出 ;


将 launch_background.xml 设置为如下配置 , 打开 第二个 item 注释 , 然后配置一个图片 ;

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- Modify this file to customize your launch splash screen -->
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@android:color/white" />

    <!-- You can insert your own image assets here -->
     <item>
        <bitmap
            android:gravity="center"
            android:src="@mipmap/ic_launcher" />
    </item>
</layer-list>
```

注意 , 有 $2$ 个 launch_background.xml 配置文件 , 都需要修改 , 不要漏掉 ;

![在这里插入图片描述](https://img-blog.csdnimg.cn/52064bd55bd642e590c5709be659518a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA6Z-p5puZ5Lqu,size_20,color_FFFFFF,t_70,g_se,x_16)



**Flutter 的启动变成下面的样式 :** 在 Flutter 渲染完成之前 , 显示一张图像 ; 这里也可以显示动画 ;


![请添加图片描述](https://img-blog.csdnimg.cn/d0df90ec9acc4ed08dc63933e324e7ce.gif)







<br>
<br>
<br>
<br>

# 三、博客源码

---

<br>


**GitHub :** [https://github.com/han1202012/flutter_startup](https://github.com/han1202012/flutter_startup)

