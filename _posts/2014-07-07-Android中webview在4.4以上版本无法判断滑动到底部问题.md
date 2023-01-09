---
layout: mypost
title: Android中webview在4.4以上版本无法判断滑动到底部问题
categories: [Android, 技术]
---
项目中有个需求，要判断webview滑动到底部的时候就显示某个控件，向上滑动就隐藏，然后滑动到顶部就显示另外一个控件。

这时候就得判断webview是否滑动到底部和顶部了：

1，我们需要重新webview的滑动方法，自定义一个webview：

```

import android.content.Context;

import android.util.AttributeSet;

import android.webkit.WebView;

/**

* webview

* @author tmacsky

*/

public class FoundWebView extends WebView {

   ScrollInterface web;

   public FoundWebView(Context context) {

       super(context);

       // TODO Auto-generated constructor stub

   }

   public FoundWebView(Context context, AttributeSet attrs, int defStyle) {

       super(context, attrs, defStyle);

   }

   public FoundWebView(Context context, AttributeSet attrs) {

       super(context, attrs);

       // TODO Auto-generated constructor stub

   }

   @Override

   protected void onScrollChanged(int l, int t, int oldl, int oldt) {

       super.onScrollChanged(l, t, oldl, oldt);

       //Log.e("hhah",""+l+" "+t+" "+oldl+" "+oldt);

       web.onSChanged(l, t, oldl, oldt);

   }

   public void setOnCustomScroolChangeListener(ScrollInterface t){

       this.web=t;

   }

   /**

    * 定义滑动接口

    * @param t

    */

   public interface ScrollInterface {

       public void onSChanged(int l, int t, int oldl, int oldt) ;

   }

}

```

2,在activity中onCreate调用

```

private FoundWebView mWebView;

mWebView.setOnCustomScroolChangeListener(new ScrollInterface() {

           @Override

           public void onSChanged(int l, int t, int oldl, int oldt) {

               // TODO Auto-generated method stub

               float webcontent = mWebView.getContentHeight()*mWebView.getScale();//webview的高度

               float webnow = mWebView.getHeight()+ mWebView.getScrollY();//当前webview的高度

               if( mWebView.getContentHeight()* mWebView.getScale() -( mWebView.getHeight()+ mWebView.getScrollY())==0){  

                   //已经处于底端  

                   lay_bottom_layout.setVisibility(View.VISIBLE);

               }else {

                   lay_bottom_layout.setVisibility(View.GONE);

               }

               //已经处于顶端

               if (mWebView.getScaleY() == 0) {

                   

               }

           }

});
```


3，此时很蛋疼的一个问题，在4.4及以上的系统上我发现无论如何获取的高度都会成这样

  ` (int)webviewHight > mWebView.getHeight() + mWebView.getScrollY()`

测试后发现某些手机上总是相差为1，没办法，后来我就直接做2者的差值在10以内就表示滑动到底部了，完美解决！


```
 mWebView.setOnCustomScroolChangeListener(new ScrollInterface() {

           @Override

           public void onSChanged(int l, int t, int oldl, int oldt) {

               // TODO Auto-generated method stub

               webviewHight = mWebView.getContentHeight()*mWebView.getScale();

               //为解决4.4的系统无法获取正确的高度加一个“<10”的

               if((int)webviewHight - (mWebView.getHeight() + mWebView.getScrollY()) < 10){  

                   //已经处于底端 10个偏移量

                   

               }

               //已经处于顶端

//                if (mWebView.getScaleY() == 0) {

//                  

//                }

           }

       });
```
