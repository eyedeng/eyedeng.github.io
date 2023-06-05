Android MVVM 架构

## 依赖注入 DI

类通常需要引用其他类。例如，`Car` 类可能需要引用 `Engine` 类。这些必需类称为依赖项

三种方式获取所需的对象：

1. 类构造其所需的依赖项。缺点：两者密切相关，强依赖

   ```kotlin
   class Car {
     private val engine = Engine()
     fun start() {
       engine.start()
     }
   }
   
   Car().start()
   ```

2. 从其他地方抓取。某些 Android API（如 `Context` getter 和 `getSystemService()`）的工作原理便是如此。

   ```kotlin
   class MyFragment : Fragment() {
     fun myFun() {
       context?.getSystemService(Context.AUDIO_SERVICE)
     }
   }
   ```

3. 以参数形式提供。应用可以在构造类时提供这些依赖项，或者将这些依赖项传入需要各个依赖项的函数。优点：可重用 `Car`。可以将 `Engine` 的不同实现传入 `Car`。

   ```kotlin
   class Car(private val engine: Engine) {
     fun start() {
       engine.start()
     }
   }
   
   class ElectricEngine : Engine() {
       
   }
   
   val engine = Engine()
   val engine2 = ElectricEngine()
   val car = Car(engine2)
   ```

   

Android 中有两种主要的依赖项注入方式：构造函数注入、字段注入（`car.engine = Engine()`）。

自动依赖项注入替代手动依赖注入，两种方式：

1. 反射，运行时连接依赖项。
2. 静态解决方案，可生成在编译时连接依赖项的代码。（库： [Dagger](https://developer.android.com/training/dependency-injection/dagger-basics?hl=zh-cn) 、基于Dagger的[Hilt](https://developer.android.com/training/dependency-injection/hilt-android?hl=zh-cn) ）