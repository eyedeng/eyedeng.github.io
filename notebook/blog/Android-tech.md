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



