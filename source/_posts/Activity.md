---
title: Activity生命周期
date: 2016/5/15 10:25:31
tags:
- Android
- Acticity
- 生命周期
categories: Android
---
### 什么是Activity？ ###
简单的说：Activity就是布满整个窗口或者悬浮于其他窗口上的交互界面。在一个应用程序中通常由多个Activity构成，都会在Manifest.xml中指定一个主的Activity，如下设置

    <actionandroid:name="Android.intent.action.MAIN" />

当程序第一次运行时用户就会看这个Activity，这个Activity可以通过启动其他的Activity进行相关操作。当启动其他的Activity时这个当前的这个Activity将会停止，新的Activity将会压入栈中，同时获取用户焦点，这时就可在这个Activity上操作了。都知道栈是先进后出的原则，那么当用户按Back键时，当前的这个Activity销毁，前一个Activity重新恢复。<!-- more -->

### Activity生命周期 ###
先看下图：
![](http://7xu37n.com1.z0.glb.clouddn.com/Activity.jpg)

这个图不再多说什么，下面我们通过一个实例来说明问题。新建工程，编写如下代码：

    package com.android.actiitylifedemo;  
    import android.app.Activity;  
    import android.os.Bundle;  
    import android.util.Log;  
    import android.view.KeyEvent;  
    public class ActivityLifeDemo extends Activity {  
    private final static String TAG="ActivityLifeDemo";  
      
    @Override  
    public void onCreate(Bundle savedInstanceState) {  
    super.onCreate(savedInstanceState);  
    setContentView(R.layout.main);  
      
    Log.i(TAG, "onCreate");  
    }  
    @Override  
    protected void onStart() {  
    Log.i(TAG, "onStart");  
    super.onStart();  
    }  
    @Override  
    protected void onRestart() {  
    Log.i(TAG, "onRestart");  
    super.onRestart();  
    }  
    @Override  
    protected void onResume() {  
    Log.i(TAG, "onResume");  
    super.onResume();  
    }  
    @Override  
    protected void onPause() {  
    Log.i(TAG, "onPause");  
    super.onPause();  
    }  
    @Override  
    protected void onStop() {  
    Log.i(TAG, "onStop");  
    super.onStop();  
    }  
    @Override  
    protected void onDestroy() {  
    Log.i(TAG, "onDestroy");  
    super.onDestroy();  
    }  
    } 
 
代码很简单，只涉及到一个Activity，一些用户的操作，我们通过记录操作和打印日志的方式来看看Activity的生命周期过程。

1、  运行
看到如下打印日志：
08-31 08:46:53.916: INFO/ActivityLifeDemo(312): onCreate
08-31 08:46:53.916: INFO/ActivityLifeDemo(312): onStart
08-31 08:46:53.916: INFO/ActivityLifeDemo(312): onResume
2、按下返回按键： 
08-31 09:29:57.396: INFO/ActivityLifeDemo(354): onPause
08-31 09:29:58.216: INFO/ActivityLifeDemo(354): onStop
08-31 09:29:58.216: INFO/ActivityLifeDemo(354): onDestroy
3、长按Home键，弹出最近打开过的应用程序，点击ActivityLifeDemo
08-31 08:51:46.916: INFO/ActivityLifeDemo(312): onCreate
08-31 08:51:46.916: INFO/ActivityLifeDemo(312): onStart
08-31 08:51:46.936: INFO/ActivityLifeDemo(312): onResume
4、按Home键
08-31 08:53:32.676: INFO/ActivityLifeDemo(312): onPause
08-31 08:53:33.796: INFO/ActivityLifeDemo(312): onStop
5、在AllList中点击打开
08-31 08:54:14.286: INFO/ActivityLifeDemo(312): onRestart
08-31 08:54:14.286: INFO/ActivityLifeDemo(312): onStart
08-31 08:54:14.296: INFO/ActivityLifeDemo(312): onResume

通过日志信息，我们可以看到。Activity的启动过程：onCreate—onStart—onResume；下返回键时：onPause—onStop—onDestroy 正如上面说是，当按下返回键时，此Activity弹出栈，程序销毁。确实如此，我们再次 打开时的启动过程又回到onCreate—onStart—onResume。OK，启动之后按下Home键，回到Launcher，查看打印信息：onPause—onStop，再次打开的运行过程：onRestart—onStart—onResume。

我们通过对Activity的各种操作，构成了Activity的生命周期，我们看到无论对Activity做如何的操作，都会接收到相关的回调方法，那么我们在开发的过程中通过这些回调方法就可以写工作，比如说释放一些重量级的对象，网络连接，数据库连接，文件读等等。

以下是各个方法的详细说明：

onCreate()：当 activity 第一次创建时会被调用。在这个方法中你需要完成所有的正常静态设置 ，比如创建一个视图（ view ）、绑定列表的数据等等。如果能捕获到 activity 状态的话，这个方法传递进来的 Bundle 对象将存放了 activity 当前的状态。调用该方法后一般会调用 onStart() 方法。

onRestart()：在 activity 被停止后重新启动时会调用该方法。其后续会调用 onStart 方法。

onStart()à当 activity 对于用户可见前即调用这个方法。如果 activity回到前台则接着调用 onResume() ，如果 activity 隐藏则调用onStop()

onResume()：在 activity 开始与用户交互前调用该方法。在这时该activity 处于 activity 栈的顶部，并且接受用户的输入。其后续会调用 onPause() 方法。

onPause()：在系统准备开始恢复其它 activity 时会调用该方法。这个方法中通常用来提交一些还没保存的更改到持久数据 中，停止一些动画或其它一些耗 CPU 的操作等等。无论在该方法里面进行任何操作，都需要较快速完成，因为如果它不返回的话，下一个 activity 将无法恢复出来。如果 activity 返回到前台将会调用 onResume() ，如果 activity 变得对用户不可见了将会调用onStop() 。

onStop()：在 activity 对用户不可见时将调用该方法。可能会因为当前 activity 正在被销毁，或另一个 activity （已经存在的activity 或新的 activity ）已经恢复了正准备覆盖它，而调用该方法。如果 activity 正准备返回与用户交互时后续会调用onRestart ，如果 activity 正在被释放则会调用 onDestroy 。

onDestroy()：在 activity 被销毁前会调用该方法。这是 activity 能接收到的最后一个调用。可能会因为有人调用了 finish 方法使得当前activity 正在关闭，或系统为了保护内存临时释放这个 activity的实例，而调用该方法。你可以用 isFinishing 方法来区分这两种不同的情况。

### 如何启动一个新的Activity？ ###
要启动一个新的Activity，我们可以通过调用Context中的startActivity来启动。像这样：

    Intent intent = new Intent(this, ActivityDemo.class);  
    startActivity(intent);  // ActivityDemo是需要启动的Activity类  
 
通过上面的方法可以启动新的Activity了，但如果我要从当前的Activity中传递数据到新的Activity呢？很简单：

    Intent intent = new Intent(this,ActivityDemo.class);  
    Bundle bundle = new Bundle();  
    bundle.putBoolean("bool_key", true);  
    intent.putExtras(bundle);  
    startActivity(intent); 
 
还有，有时候我们需要启动带返回值的Activity，简单的说就是需要新启动的Activity返回时将值传递给启动它的Activity，像这样：


    Intent intent = new Intent(ActivityLifeDemo.this,RevalueActivity.class);  
    startActivityForResult(intent, 0x1001);  

ActivityLifeDemo是当前的Activity，启动RevalueActivity，我们在ActivityLifeDemo中需要获取RevalueActivity传回来的值。那么在RevalueActivity中就必须这样写：

    Intent intent  = new Intent();  
    intent.putExtra("revalue_key","haha-revalueActivity");  
    setResult(0x1001, intent);

那么“revalue_key”值在哪里获取呢？必须重写onActivityResult方法，通过判断requestCode，来确定

    if(requestCode==0x1001){  
       String str = data.getStringExtra("revalue_key");  
       Log.i(TAG, "返回的值为："+str);  
    }  

### 保存Activity运行状态 ###
通过重写onSaveInstanceState（）方法来实现Activity的运行状态，请注意以下几点：

1）由于activity 对象被暂停或停止时，它仍然保留在内存里面，关于它的成员信息和当前状态都是活动的，所以此时可以保存Activity的状态，从而使用户所作的Activity的更改保存在内存中

2）  当系统回收内存而将Activity销毁时，就无法保存其状态，所以需要调用onSaveInstanceState（）方法来实现状态的保存

3）  很多情况并不需要保持状态信息，比如按下返回键直接关闭程序，所以并不能保证会调用onSaveInstanceState。如果调用了该方法，一般是在onStop 方法之前且可能在 onPause 之后调用。尽管如此，即使你没做任何操作或没有实现 onSaveInstanceState() 方法，你的 activity 状态也能通过Activity 类里面默认实现的 onSaveInstanceState 方法恢复出来。特别是会为布局中的视图（ View ）默认调用onSaveInstanceState 方法，并在这个方法中允许每一个视图提供它需要恢复的任何信息。几乎每一个 Android框架中的 widget 都视情况实现了这个方法。

注：因为 onSaveInstanceState 方法不一定会被调用，所以你应该只是用它来保存一些 activity 的转换过程状态（即 UI 的状态），而不能用来保存永久性数据。但你可以用 onPause 方法在用户离开 activity 时来保存永久性数据，比如需要保存到数据库的数据。

有一个很好的方法可以用来检验应用程序保存状态的能力，就是简单地旋转你的设备来改变屏幕的方向。因为当屏幕方向改变时，系统为了给新的方向提供一个可能合适的代替资源，会销毁 activity 并新建一个新的。由于这个原因，你的 activity 是否能在其重新创建时完成保存状态就显得尤为重要，因为用户经常会在使用应用程序时旋转屏幕的。


### 完全退出程序 ###
通过上面的介绍，我们知道当点击back键时，程序调用了onDestroy方法，程序退出了，但是我们查看其进程，发现调用了onDestroy方法之后这个Activity还在运行。甚至调用了finish()方法之后程序还能在进程中看到。通过下面这种方式可以实现程序的完全退出：

    Intent intent = new Intent();  
    Intent.setClass(context,MainActivity.class);  
    intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);  
    intent.putExtra(“flag”,EXIT_APPLICATION);  
    context.startActivity(intnet);   
