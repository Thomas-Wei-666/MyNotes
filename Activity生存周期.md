# Activity生存周期

- 预备概念：

  - 返回栈：

    Android是用`Task`来管理`Activity`的，一个`Task`是一组存放在栈里面的`Activity`的集合，这个栈被称为返回栈。

    在不做任何设置的情况下，启动`Activity`，入栈并处于栈顶的位置；按返回键/调用`finish()`，出栈。
    系统显示栈顶的`Activity`。

  - 回调：

    > 假设你公司的总经理出差前需要你帮他办件事情，这件事情你需要花些时间去做，这时候总经理肯定不能守着你做完再出差吧，于是就他告诉你他的手机号码叫你如果事情办完了你就打电话告诉他一声
	`onActivityResult()` `onRequestPermissionsResult()`

  - `Activity`的状态：

    - 运行状态：

      位于返回栈栈顶，无遮挡，不会被回收。

    - 暂停状态：

      不再位于栈顶，被部分遮挡（仍然可见），不会被回收。

    - 停止状态：

      不再位于栈顶，被全部遮挡（不可见），可能被回收。

    - 销毁状态：

      已从返回栈中移除，优先回收。

- 生存周期：

  ![ActivityLife](C:\Users\28942\Downloads\ActivityLife.png)

其中：

`onCreate()`在`Activity`第一次创建的时候调用 ；`onDestroy()`在被销毁之前调用。

`onStart()`在`Activity`从不可见变为可见的时候调用 ； `onStop()`在从可见变为不可见的时候调用。

`onResume()`在`Activity`准备好和用户进行交互的时候调用；`onPause()`在`Activity`在栈中的位置下调时调用。



`onRestart()` 在`Activity`从停止状态重新变为运行状态的时候调用。 