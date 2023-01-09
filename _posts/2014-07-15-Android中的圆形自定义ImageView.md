---
layout: mypost
title: Android中的圆形自定义ImageView
categories: [Android]
---

![touxiang](https://imglf4.lf127.net/img/YzdkeVlmZUkrV2xYcWppaTVTUFBaM29YSDZ4UXRSMUVGYWl1bS9mS083Rmd1Njd0Lzg0bGJRPT0.jpg)

一看到这个头像坑定是重写imageview，然后给imageview外面画一个环，后来我在重写方法onDraw()里画的圈圈一直都是描边而非头像上的间隔，百思不得骑姐啊，损失一个多小时在网上找各种解决方法，最后还是搞不定，然后突然我发现我犯了一个极其愚蠢的错误，其实这个头像完全可以分解成2个imageview，一个直接设置背景为一个透明的圆环，另外一个设置自定义的RoundImageView，问题就引刃而解了，很多时候换一个角度其实能更快的达到目的！

------------------------------以下是一个圆角imageview的自定义view------------------------------------

 
```
import android.content.Context;

import android.graphics.Canvas;

import android.graphics.Color;

import android.graphics.Paint;

import android.graphics.PorterDuff;

import android.graphics.PorterDuffXfermode;

import android.graphics.RectF;

import android.util.AttributeSet;

import android.view.MotionEvent;

import android.widget.ImageView;

/**

* 圆角矩形自定义图

* @author tmacsky

*/

publicclass RoundImageView extends ImageView{

   public RoundImageView(Context context, AttributeSet attrs) {

       super(context, attrs);

       init();

   }

   public RoundImageView(Context context) {

       super(context);

       init();

   }

   privatefinal RectF roundRect = new RectF();

   privatefloatrect_adius = 180;//圆角矩形弧度，180表示圆形

   privatefinal Paint maskPaint = new Paint();

   privatefinal Paint zonePaint = new Paint();

   privatevoid init() {

       maskPaint.setAntiAlias(true);

       maskPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.SRC_IN));

       //

       zonePaint.setAntiAlias(true);

       zonePaint.setColor(Color.WHITE);

       //

       float density = getResources().getDisplayMetrics().density;

       rect_adius = rect_adius * density;

   }

   publicvoid setRectAdius(float adius) {

       rect_adius = adius;

       invalidate();

   }

   @Override

   protectedvoid onLayout(boolean changed, int left, int top, int right,

           int bottom) {

       super.onLayout(changed, left, top, right, bottom);

       int w = getWidth();

       int h = getHeight();

       roundRect.set(0, 0, w, h);

   }

   @Override

   publicvoid draw(Canvas canvas) {

       canvas.saveLayer(roundRect, zonePaint, Canvas.ALL_SAVE_FLAG);

       canvas.drawRoundRect(roundRect, rect_adius, rect_adius, zonePaint);

       //

       canvas.saveLayer(roundRect, maskPaint, Canvas.ALL_SAVE_FLAG);

       super.draw(canvas);

       canvas.restore();

   }

   @Override

   publicboolean onTouchEvent(MotionEvent event) {

       // TODO Auto-generated method stub

       returnsuper.onTouchEvent(event);

   }

}
```
 

该自定义的view直接当做imageview调用，无需任何配置，简单好用

