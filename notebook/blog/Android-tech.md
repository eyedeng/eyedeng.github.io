遇到的Android相关知识点的记录：

- cheatsheet
- 需要按图索骥持续了解的

### 动画

- View Animation 包括 Tween Animation（补间动画）：scale、rotate等 和 Frame Animation(逐帧动画)；
- Property Animator 包括 ValueAnimator 和 ObjectAnimation；
view anim 作用于视图控件，而Prop anim作用于控件属性

### 颜色

不透明1 -> 全透明0
alpha 完全透明 0

### Fragment

[谈谈Fragment中的onActivityResult](https://www.cnblogs.com/tangZH/p/5930491.html)

在Fragment中使用startActivityForResult之后，onActivityResult的调用是从activity中开始的（即会先调用activity中的onActivityResult）。

一.只嵌套了一层Fragment（比如activity中使用了viewPager，viewPager中添加了几个Fragment）
在这种情况下要注意几个点：

1. 在Fragment中使用startActivityForResult的时候，不要使用getActivity().startActivityForResult,而是应该直接使startActivityForResult()。
2. 如果activity中重写了onActivityResult，那么activity中的onActivityResult一定要加上super.onActivityResult(requestCode, resultCode, data)。

如果违反了上面两种情况，那么onActivityResult只能够传递到activity中的，无法传递到Fragment中的。
没有违反上面两种情况的前提下，可以直接在Fragment中使用startActivityForResult和onActivityResult，和在activity中使用的一样。

### 广播

发广播：sendBroadcast(Intent(Action))
系统广播
自己发自定义广播
接收：

- 静态注册`<receiver <intent-filter>>`
- registerReceiver(BroadcastReceiver, IntentFilter)



### RN 

原生模块
供JS调用
extends ReactContextBaseJavaModule

@ReactMethod
eg: void encodeMD5(String content, Callback callback)
  回调函数

注册模块
implements ReactPackage
add( packages add ( modules ) )

为了让你的功能从 JavaScript 端访问起来更为方便，通常我们都会把原生模块封装成一个 JavaScript 模块（非必须）

发送事件到JS

```java
reactContext
    .getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
    .emit(eventName, params);
```

Add the listener for onActivityResult
reactContext.addActivityEventListener(mActivityEventListener);

监听生命周期
reactContext.addLifecycleEventListener(this);

原生UI组件

### 组件化
### 多工程

### WebView

设置了自定义的WebViewClient，应用就会从默认调用外部浏览器打开网址变为默认在本地WebView上打开网址。

shouldOverrideUrlLoading(WebView view, String url)使用：
默认返回false，想控制权交给app时返回true，可解析url重定向（处理url）。

### 注意

recyclerview bindView时，if else两种情况都要处理，否则ui被覆盖

```java
if (song.needShowSource()) {
    musicSource.setVisibility(View.VISIBLE);
    musicSource.setImageResource(song.getSourceImgRes());
} else {
    musicSource.setVisibility(View.GONE);
}
```



### 文件存储

| Tag |        对应的路径 | 方法 |
| --- | --- | --- |
root-path    |    根目录/
files-path   |     /data/user/0/<package_name>/files 或者/data/data/<package_name>/files | context.getFileDir() |
cache-path    |    /data/user/0/<package_name>/cache 或者 /data/data/<package_name>/cache | context.getCacheDir() |
external-path |       /storage/emulated/0或者/sdcard/ | getExternalStorageDirectory() |
external-files-path |        /storage/emulated/0/Android/data/<package_name>/files 或者 /sdcard/Android/data/<package_name>/files | getExternalFilesDir |
external-cache-path |      /storage/emulated/0/Android/data/<package_name>/cache 或者 /sdcard/Android/data/<package_name>/cache | getExternalCacheDir() |

### 媒体播放

[媒体应用架构概览](https://developer.android.com/guide/topics/media-apps/media-apps-overview?hl=zh-cn)

**媒体控制器**会隔离您的**界面**。您的界面代码只与媒体控制器（而非**播放器**本身）通信。媒体控制器会将**传输控制操作**转换为对媒体会话的回调（session callbacks）。每当会话状态发生变化时，它也会接收来自媒体会话的回调。

### 反编译

工具：jadx

其它工具：
1. 如果要反编译 app 的代码，就使用 dex2jar + jd-gui

2. 如果要反编译 app 的资源，就使用 apktool

### 启动模式

所以在 Service 中启动 Activity 必须添加 FLAG_ACTIVITY_NEW_TASK，原因也很简单，每个 Activity 启动都需要一个任务栈，非 Activity 的 context 存在后台启动的可能，而此时前台是其他 App 的任务栈，甚至我们的 App 根本没有创建过任务栈，为了防止这些无法预料的情况出现，被强制要求添加这个 Flag。

### 启动Activity获取返回结果

新方式：

```java
private val activityForResult =
  registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result ->
  		result.resultCode, result.data
  }

activityForResult.launch(intent)
```

### rxjava2

flatmap 

### RecyclerView

在`onAttachedToRecyclerView` 调整列表项的跨度大小（spanSize），若总列数（spanCount）为2，spanSize为1，UI表现为item分两列。

```kotlin
// file 1 activity
mBinding.feedbackRvDeviceList.run {
    val spanCount = 2
    val layoutManager = GridLayoutManager(this@FeedBackDeviceActivity, spanCount)
    setLayoutManager(layoutManager)
    mDeviceAdapter = FeedBackDeviceAdapter(mDataList, context)
    adapter = mDeviceAdapter
}

// file 2 adapter
override fun onAttachedToRecyclerView(recyclerView: RecyclerView) {
    super.onAttachedToRecyclerView(recyclerView)
    val manager = recyclerView.layoutManager as GridLayoutManager?
    if (manager != null) {
        manager.spanSizeLookup = object : GridLayoutManager.SpanSizeLookup() {
            override fun getSpanSize(position: Int): Int {
                when (getItemViewType(position)) {
                    TYPE_LABLE -> return manager.spanCount
                    TYPE_DATA -> return 1
                    TYPE_APP -> return 2
                    TYPE_BOTTOM -> return 2
                    else -> return 1
                }
            }
        }
    }
}


```

### UI

###### RecyclerView列表单选

思路：`selectedPosition`，用于保存当前选中项的位置，点击下一项时，`notifyItemChanged`刷新这两项

###### 字母索引侧边栏

###### 图片

`ImageView控件`和`IV里的图片`的长宽和比例是独立的

ImageView scaleType [图解](https://www.jianshu.com/p/32e335d5b842)

adjustViewBounds只有在ImageView一边固定，一边为wrap_content的时候才有意义。设置为true的时候，可以让ImageView的比例和原始图片一样，以达到让图片充满ImageView的效果。

### 权限

### H5

```
addJavascriptInterface(JsBridge, name)
```

Cookie注入，参数url可以是domain

```
domain = ".abc.com"
cookieManager.setCookie(url, "$name=$value; path=/; domain=$domain")
```

```
    public boolean shouldOverrideUrlLoading(WebView view, String url) {
        if (url.startsWith("myscheme")) {
            return true;  // 拦截处理
        }
        return super.shouldOverrideUrlLoading(view, url);  // false 让WebView正常加载
    }
```

### 文件

log、图片、zip，创建、写入

上传、下载文件

功能：图片选择展示上传、日志上传

### 内存泄漏

常见原因、场景：

- Static activity、Static view

- 单例中保存activity，因为单例生命周期大于activity生命周期

  new Singleton(context) 改为 new Singleton(context.getApplicationContext())

- Inner Class、Anonymous Class，因为内部类持有外部类的强引用

  改为静态内部类、弱引用activity

弱引用。在GC时，只具有弱引用的对象内存会被回收

```java
WeakReference<Context> contextWeakReference = new WeakReference<>(context);
contextWeakReference.get();
```

### ContentProvider

跨进程共享数据

```java
getContentResolver().call(CALL_PROVIDER_AUTHORITY, method, arg, extras)

getContentResolver().registerContentObserver(Settings.Secure.getUriFor(NAME), false, mInvisibleObserver)
        mInvisibleObserver = new ContentObserver(getMainHandler()) {
            @Override
            public void onChange(boolean selfChange) {
                
            }
        };
```

### IPC

进程间通信，方式：Binder、Socket

使用多进程唯一方法：给四大组件指定`android:process`

##### AIDL

服务端：定义AIDL文件，创建Service，实现AIDL接口。

```java
// .aidl
interface IMyService {
		void showData(in Bundle para);
}


public class MyService extends Service {
  	private Binder mBinder = new IMyService.Stub() {
      
      	@Override
        public void showData(Bundle para) throws RemoteException {
          	// 实现
        }
    }
  
  	@Override
    public IBinder onBind(Intent intent) {
        return mBinder;
    }  
  
}
```

客户端：bind Service，获取Binder转AIDL接口类型，调用方法。

```java
		private IMyService mService;
    private final ServiceConnection mConnection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
            mService = IMyService.Stub.asInterface(iBinder);
          	mService.showData(para);
        }

    };
context.bindService(intent, mConnection, Context.BIND_AUTO_CREATE);
```

### VirtualDisplay

代表一个显示器（只不过没有物理实体），应用可以在显示器上打开窗口（副屏镜像），系统可以将其他显示器的内容镜像到它上面（主屏镜像）。显示器内容被渲染到传给Dispaly的Surface。

```java
VirtualDisplay createVirtualDisplay(@NonNull String name, int width, int height, int densityDpi, @Nullable Surface surface, int flags, @Nullable VirtualDisplay.Callback callback, @Nullable Handler handler) 

mVirtualDisplay.setSurface(surface)

mVirtualDisplay.resize(width, height, density)  更改屏幕尺寸
```

 MediaCodec#createInputSurface()、ImageReader#getSurface，这些类内部有Surface，以Surface作为输入。添加监听，拿到这些类处理Surface的数据（视频流、图像）。

### 反射

只知道类的某些定义：类名、方法，实例未知，通过反射调用其方法。

```java
interface ITaskManager {
  void removeTask(int taskId);
  
}

// 某个未知的实例
static ITaskManager getInstance() {
  return new ITaskManager() {
    @Override
    public void removeTask(int taskId) {
      System.out.println("remove " + taskId);
    }
  };
}

static Method ITaskManager_removeTask;

try {
  ITaskManager_removeTask = ITaskManager.class.getDeclaredMethod("removeTask", int.class);
} catch (NoSuchMethodException e) {

}

try {
  ITaskManager_removeTask.invoke(getInstance(), taskId);
} catch (Exception e) {

}


```

#### 屏幕方向

### 音视频

##### 采集 

Audio：AudioRcorder，裸数据PCM

Video：摄像头/屏幕数据 -> Texture -> Surface -> 预览

- `Texture`（纹理）是 **存储图像数据的 GPU 资源**，可以用来渲染画面
- `Surface` 是 **Android 界面系统的基本绘制单元**，可以接收 GPU 渲染的数据，供 Display 显示
- `Display` 代表 **可以显示图像的输出设备**，不直接持有图像数据，通过 Surface 获取

##### 编码

`MediaCodec`

##### 播放

`AudioTrack`

### 输入

MotionEvent extends InputEvent  屏幕触摸事件

