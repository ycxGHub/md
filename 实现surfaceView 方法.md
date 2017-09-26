## 实现surfaceView 方法：

1. 需要继承surfaceView:**

>并且实现:surfaceHolder.Callback
>```java
>public void surfaceChanged(SurfaceHolder holder,int format,int width,int height){}
>//在surface的大小发生改变时激发
>public void surfaceCreated(SurfaceHolderholder){}
>//在创建时激发，一般在这里调用画图的线程。
>public void surfaceDestroyed(SurfaceHolder holder) {}
>//销毁时候激发
>```

2.  销毁时激发，一般在这里将画图的线程停止、释放。整个过程：

>继承SurfaceView并实现SurfaceHolder.Callback接口 ----> SurfaceView.getHolder()获得SurfaceHolder对象 ---->SurfaceHolder.addCallback(callback)添加回调函数---->SurfaceHolder.lockCanvas()获得Canvas对象并锁定画布----> Canvas绘画 ---->SurfaceHolder.unlockCanvasAndPost(Canvas canvas)结束锁定画图，并提交改变，将图形显示。

具体代码如下:

```java
package adr.ycx.com.myapplication;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.support.annotation.Nullable;
import android.util.AttributeSet;
import android.view.SurfaceHolder;
import android.view.SurfaceView;
import android.view.WindowManager;

import java.util.ArrayList;

/**
 * 创建日期：2017/9/22
 * 创建者ycx
 */

public class CircleViewBS extends SurfaceView implements SurfaceHolder.Callback {
    private Paint pathPaint;
    private int currentX;
    private int currentY;
    private MyThread drawThread;

    public CircleViewBS(Context context) {
        super(context);
        pathPaint = new Paint();
        WindowManager wm = (WindowManager) getContext().getSystemService(Context.WINDOW_SERVICE);
        currentX = wm.getDefaultDisplay().getWidth();
        currentY = wm.getDefaultDisplay().getHeight();
        pathPaint.setStrokeWidth(5);
        pathPaint.setAntiAlias(true);
        pathPaint.setStyle(Paint.Style.STROKE);//只描绘边长
        pathPaint.setColor(Color.BLACK);
        SurfaceHolder holder = this.getHolder();
        holder.addCallback(this);
        drawThread = new MyThread(holder);//创建一个绘图线程
    }

    public CircleViewBS(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }

    public CircleViewBS(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    public CircleViewBS(Context context, @Nullable AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
    }

    @Override
    public void surfaceCreated(SurfaceHolder holder) {
        drawThread.start();
    }

    @Override
    public void surfaceChanged(SurfaceHolder holder, int format, int width, int height) {

    }

    @Override
    public void surfaceDestroyed(SurfaceHolder holder) {
        drawThread.stopRun();
    }

    public class CircleData {
        private final static float maxRadius = 220;
        private final static int minalpha = 5;
        private final static int nums = 100;
        public float radius = 20;
        public int alpha = 255;
        public int state = 0;

        public void refreshState() {
            if (state > 255) {
                state = 0;
                radius = 20;
                alpha = 250;
            }
            radius = 20 + 1 * state;
            alpha = 255 - 1* state;
            state++;
        }

        public void setState(int state) {
            this.state = state;
        }

        public int getalpha() {
            return alpha;
        }

        public float getRadius() {
            return radius;
        }
    }

    private class MyThread extends Thread {
        private SurfaceHolder mSurfaceHolder;
        private boolean isRun = true;

        public MyThread(SurfaceHolder surfaceHolder) {
            mSurfaceHolder = surfaceHolder;
        }

        Canvas mCanvas = null;
        float radius = 10;
        int alpha = 255;
        float maxradius = 100;
        int minalpha = 0;
        ArrayList<CircleData> circleList = new ArrayList<>();
        int i = 0;

        @Override
        public void run() {
            super.run();
            while (isRun) {
                try {
                    mCanvas = mSurfaceHolder.lockCanvas();
                    mCanvas.drawColor(Color.WHITE);
                    if (i < 5) {
                        int state = 0;
                        switch (i) {
                            case 0:
                                state = 0;
                                break;
                            case 1:
                                state = 51;
                                break;
                            case 2:
                                state = 102;
                                break;
                            case 3:
                                state = 153;
                                break;
                            case 4:
                                state = 204;
                                break;
                        }
                        CircleData circleData = new CircleData();
                        circleData.setState(state);
                        circleList.add(circleData);
                    }
                    for (CircleData circleData : circleList) {
                        drawCircle(circleData);
                    }
                    i++;
                    if (mCanvas != null) {
                        mSurfaceHolder.unlockCanvasAndPost(mCanvas);//结束锁定画图，并提交改变。
                    }
                    Thread.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }

        public void stopRun() {
            isRun = false;
        }

        public void drawCircle(CircleData ciledat) {
            ciledat.refreshState();
            Paint paint = new Paint(pathPaint);
            paint.setAlpha(ciledat.getalpha());
            mCanvas.drawCircle(currentX / 2, currentY / 2, ciledat.getRadius(), paint);
        }
    }
}

```



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
