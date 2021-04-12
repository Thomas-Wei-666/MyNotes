#  Android广播

###  动态注册/静态注册

- 动态注册广播：

  - 发送：

    ```java
    Intent intent = new Intent("action");//action:该广播的标识，用于接收器识别
    sendBroadcast(intent);
    ```

    1. 必须要有特定的`action`
    2. 不需要`setComponent`

  - 接收：

    定义一个`MyReceiver`类

    ```java
    class MyReceiver extend BroadcastReceiver{
        @Override
        public void onReceive(Context context, Intent intent){
            //To be completed...
        }
    }
    ```

    在内部重写`onReceive`，*用于实现在接收到广播之后的操作*。

    *注意`onReceive(Context context, Intent intent)`中的`intent`参数，它可以携带数据（`putExtra()`/`get...Extra()`）。*


    将定义好的`MyReceiver`类*注册*为接收带有特定`action`的接收器：

    ```java
    IntentFilter intentFilter = new IntentFilter();
    intentFilter.addAction("action");
    //用IntentFilter存储action
    MyReceiver myReceiver = new MyReceiver();
    registerReceiver(myReceiver, intentFilter);
    //将MyReceiver实例化并将实例与intentFilter绑定
    
    //..........
        
    unregisterReceiver(myReceiver);
    myReceiver = null;
    //使用完之后一定要解除注册，并且将指针指向null
    
    ```

    P.S.:

    一般在`onResume()`中注册`Receiver`，在`onPause()`中解除注册，这样可以保证只有在返回栈栈顶的`Activity`可以接收到广播。

- 静态注册广播：

  - 发送：

    ```java
    Intent intent = new Intent("action");//action:该广播的标识，用于接收器识别，此处不必要
    intent.setComponent(new ComponentName("Receiver的包名","Receiver的包名+Receiver的文件名"));//相当于指定了由哪个Receiver接收
    sendBroadcast(intent);
    ```

    1. `action`不再必要
    2. 一定要`setComponent`

  - 接收：

    注册`Receiver`步骤（AS辅助）：

    1. 在某`package`下点击`New`，选择`Other`中的`Broadcast Receiver`
    2. 点击创建
    3. （选做）在`Manifest`文件里面的`<receiver`目录下添加`<intent-filter`并在里面添加`<action : ...`选项

    静态注册`Receiver`是需要在`Manifest`文件里面注册，执行以上步骤可让AS帮助我们在`Manifest`中注册。

    生成文件格式和动态注册中的格式一样。

  ### 有序广播/本地广播

  - 有序广播

  > 广播根据优先级由高到低有顺序的在接收器之间传播，排在前面的接收器可以决定是否阻断该广播的传播。

  e.g.:

  ```java
  //Manifest:定义优先级
  <receiver 
      ...
      <intent-filter android : priority = 100//优先级，数字越大优先级越高。
          .....
          <intent-filter/>
  //---------------------------------------------------------------------------------
  //在onReceive函数中可以用
          abortBroadcast();
  //来终止传播
  ```

- 本地广播：

  > 本地广播指该广播只有发出该广播的应用能够接收到。

  发送和接收基本与普通广播无异，只是均使用`LocalbroadcastManager`类中的同名函数。

  首先要将`LocalBroadcastManager`实例化：

  ```java
  LocalBroadcastManager manager = LocalBroadcastManager.getInstance(this);
  ```

  