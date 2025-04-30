
# livedata常用问题

## 背景 
livedata是一个带泛型的数据容器组件，里边可以保存任何引用类型的数据。结合观察者模式，他能在数据返回时自动通知回调用（常见为ui更新）。结合activity的生命周期，实现了订阅者的自动反注册。

## livedata常见问题

livedata经常和viewModel搭配来实现android的mvvm架构。在使用的过程中有一些需要注意的问题

- livedata的粘性事件

每个livedata都会控制一个version，没进行一次通知version+1，当observer手动通知之后会判断lastversion和version的大小，**version更大**才会处理通知
但是如果livedata作为发送方，早于观察者的初始化，观察者初始化的时的lastversion为-1肯定小于verison，所以会执行回调

- livedata如何实现反注册

livedata加入新的observer时，也会一并加入到lifeCycleOwner，此后会监听到activity的生命周期，当activity进入onDestory时，自动执行反注册，避免了内存泄漏

- 什么是livedata的数据倒灌

当activity重建的时候，oncreate生命周期重新执行，会导致livedata注册新的observer，由于lastversion的原因，每次重建都会重新执行一次回调。

- setValue和postValue的区别

setValue在主线程使用，postValue在可在子线程使用，但本质还是通过handler抛到了主线程。postValue连续调用多次，在runnable未执行之前，消息队列里边只会有一个runnable，但是penddingData会不断更新

