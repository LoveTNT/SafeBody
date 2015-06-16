# SafeBody
SafeBody

Hello___World

Minimum  Required  SDK 是指程序最低兼容的版本，这里我
们选择 Android 4.0。Target SDK 是指你在该目标版本上已经做过了充分的测试，系统不会再
帮你在这个版本上做向前兼容的操作了，这里我们选择最高版本 Android 4.4。Compile With
是指程序将使用哪个版本的 SDK 进行编译，这里我们同样选择 Android 4.0。最后一个 Theme
是指程序 UI 所使用的主题，我个人比较喜欢选择 None。

 AndroidManifest.xml 
这是你整个 Android 项目的配置文件，你在程序中定义的所有四大组件都需要在这
个文件里注册。另外还可以在这个文件中给应用程序添加权限声明，也可以重新指定你
创建项目时指定的程序最低兼容版本和目标版本。


 Android 程序的设计讲究逻辑和视图分离
 是不推荐在活动中直接编写界面的，
更加通用的一种做法是，在布局文件中编写界面，然后在活动中引入进来。你可以看到，在
onCreate()方法的第二行调用了 setContentView()方法，就是这个方法给当前的活动引入了一
个 hello_world_layout 布局


。Android
不 推 荐 在 程 序 中 对 字 符 串 进 行 硬 编 码 ， 更 好 的 做 法 一 般 是 把 字 符 串 定 义 在
res/values/strings.xml 里，然后可以在布局文件或代码中引用

Intent 的用法大致可以分为两种，显式 Intent 和隐式 Intent


 
 活动的生命周期

 ~~~ Android 是使用任务（Task）来管理活动
 一个任务就是一组存放在栈里的活动
的集合，这个栈也被称作返回栈（Back Stack）
栈是一种后进先出的数据结构
 Back 键或调用 finish()方法去销毁一个活动时，处于栈顶的活动会出栈，这时前一个入
栈的活动就会重新处于栈顶的位置。系统总是会显示处于栈顶的活动给用户。


 活动状态 
每个活动在其生命周期中最多可能会有四种状态

1.  运行状态 
2.  暂停状态 
3.  停止状态
4.  销毁状态 

活动的生存期 Activity 类中定义了七个回调方法

1 onCreate()  。你应该在这个方法中完成活动的初始化操作
2  onStart() 这个方法在活动由不可见变为可见的时候调用
3.  onResume()此时的活动一定位于返回栈的
栈顶，并且处于运行状态。
4.  onPause() 法在系统准备去启动或者恢复另一个活动的时候调用。我们通常会在这个方
法中将一些消耗 CPU 的资源释放掉，以及保存一些关键数据，但这个方法的执行速度
一定要快，
5.  onStop() 活动完全不可见的时候调用。它和 onPause()方法的主要区别在于，如
果启动的新活动是一个对话框式的活动，那么 onPause()方法会得到执行，而 onStop()
方法并不会执行。 
6.  onDestroy() 
这个方法在活动被销毁之前调用，之后活动的状态将变为销毁状态。



7.  onRestart() 
  onRestart() 
这个方法在活动由停止状态变为运行状态之前调用，也就是活动被重新启动了。 

从而又可以将活动分为三
种生存期。 

1.  完整生存期   onCreate()方法和 onDestroy()方法之间所经历的，就是完整生存期
2.  可见生存期 
onStart()方法和 onStop()方法之间所经历的，就是可见生存期。在可见生存
期内，活动对于用户总是可见的，即便有可能无法和用户进行交互。我们可以通过这两
个方法，合理地管理那些对用户可见的资源。比如在 onStart()方法中对资源进行加载，
而在 onStop()方法中对资源进行释放，从而保证处于停止状态的活动不会占用过多内存。
3.  前台生存期 

活动在 onResume()方法和 onPause()方法之间所经历的，就是前台生存期。在前台
生存期内，活动总是处于运行状态的，此时的活动是可以和用户进行相互的

，Activity 中还提供了一个 onSaveInstanceState()回调方法，这
个方法会保证一定在活动被回收之前调用


活动的启动模式 
启动模式一共有四种，分别是 
standard、standard 是活动默认的启动模式
singleTop、
singleTask 
singleInstance，

singleTop  singleTop 模式。当活动的启动模式
指定为 singleTop，在启动活动时如果发现返回栈的栈顶已经是该活动，则认为可以直接使用
它，不会再创建新的活动实例。
<activity 
    android:name=".FirstActivity" 
    android:launchMode="singleTop" 
    android:label="This is FirstActivity" > 
    <intent-filter> 
    
    </intent-filter> 
</activity> 



singleTask 模式来实现了。当活动的启动模式指定为 singleTask，每次启动该活动时系统首先
会在返回栈中检查是否存在该活动的实例，如果发现已经存在则直接使用该实例，并把在这
个活动之上的所有活动统统出栈，如果没有发现就会创建一个新的活动实例。
<activity 
    android:name=".FirstActivity" 
    android:launchMode="singleTask" 
    android:label="This is FirstActivity" > 
    <intent-filter> 
 </activity> 

为 singleInstance 模式的活动会启用一
个新的返回栈来管理这个活动


 ActivityCollector 类作为活动管理器
 public class ActivityCollector { 
   
  public static List<Activity> activities = new ArrayList<Activity>(); 
 
  public static void addActivity(Activity activity) { 
    activities.add(activity); 
  } 
 
  public static void removeActivity(Activity activity) { 
    activities.remove(activity); 
  } 
 
  public static void finishAll() { 
    for (Activity activity : activities) { 
      if (!activity.isFinishing()) { 
        activity.finish(); 
      } 
    } 
  } 
 
} 
通过一个 List 来暂存活动，然后提供了一个 addActivity()方法用
于向 List 中添加一个活动，提供了一个 removeActivity()方法用于从 List 中移除活动，最后
提供了一个 finishAll()方法用于将 List 中存储的活动全部都销毁掉。

public class BaseActivity extends Activity { 
 
  @Override 
  protected void onCreate(Bundle savedInstanceState) { 
    super.onCreate(savedInstanceState); 
    Log.d("BaseActivity", getClass().getSimpleName()); 
    ActivityCollector.addActivity(this); 
  } 
   
  @Override 
  protected void onDestroy() { 
    super.onDestroy(); 
    ActivityCollector.removeActivity(this); 
  } 
   
}
































  










