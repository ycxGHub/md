## 实现surfaceView 方法：

1. 需要继承surfaceView:**

>并且实现:surfaceHolder.Callback
>```java
>public void surfaceChanged(SurfaceHolder holder,int format,int width,int height){}
>//在surface的大小发生改变时激发
>public void surfaceCreated(SurfaceHolderholder){}``
>//在创建时激发，一般在这里调用画图的线程。
>public void surfaceDestroyed(SurfaceHolder holder) {}``
>//销毁时候激发
>```

2.  销毁时激发，一般在这里将画图的线程停止、释放。整个过程：

>继承SurfaceView并实现SurfaceHolder.Callback接口 ----> SurfaceView.getHolder()获得SurfaceHolder对象 ---->SurfaceHolder.addCallback(callback)添加回调函数---->SurfaceHolder.lockCanvas()获得Canvas对象并锁定画布----> Canvas绘画 ---->SurfaceHolder.unlockCanvasAndPost(Canvas canvas)结束锁定画图，并提交改变，将图形显示。

## 如何获取当前屏幕长宽 ：

android获取屏幕的高度和宽度用到**WindowManager**这个类，两种方法：
>```java
>1. WindowManager wm = (WindowManager) getContext() .getSystemService(Context.WINDOW_SERVICE)
>int width = wm.getDefaultDisplay().getWidth(); 
>int height = wm.getDefaultDisplay().getHeight();
>2.WindowManager wm = this.getWindowManager()
>int width = wm.getDefaultDisplay().getWidth();
>int height = wm.getDefaultDisplay().getHeight();
>```
