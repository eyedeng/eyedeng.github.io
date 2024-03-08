## Handler消息机制

1. 使用线程的Looper创建Handler，通过Handler的send post发送Message（post方法会把runnable封装成Message）。每个线程仅一个Looper，主线程已初始化好：Looper.getMainLooper()，开子线程需先Looper.prepare()，常用HandlerThread获取它的Looper。
2. Message会加入MessageQueue，等待Looper取出处理
3. Looper不断从MessageQueue取Message交给对应Handler处理。（通过Message查找对应Handler，Message.target）

Handler是整个消息机制的发起者与处理者。

创建Handler：继承Handler，重写handleMessage(Message)。直接传入callBack创建Handler

作用

切换代码执行线程（因为Msg放入了不同线程的Looper，在不同线程调用代码段），如子线程访问网络主线程更新UI

顺序处理消息，避免并发（因为队列结构）

延迟处理消息



## 线程

创建线程

继承Thread，重写run()。实现Runnable，重写run()

线程的状态

new、Runable：调start()后，就绪等待被cpu执行、Running：运行run()、Blocked、Dead

线程安全、同步

volatile修饰变量：确保从内存读取最新值，在多线程环境下，变量可能被其它线程修改。

```java
class MyThread extends Thread {
  private ReadableArray uuiDs;

  private volatile boolean isSendPowering = true;

  public void stopSendPowerData() {
    isSendPowering = false;
  }

  public BluetoothPowerThread(ReadableArray uuiDs) {
    this.uuiDs = uuiDs;

  }

  @Override
  public void run() {
    isSendPowering = true;
    final int MAX_COUNT = 500;
    int count = 0;
    while (isSendPowering && count < MAX_COUNT) {
      try {
        Thread.sleep(30);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      count++;
    }
  }
}

myThread.stopSendPowerData();
```

synchronized 保证同一时刻只有一个线程进入该方法或代码块

