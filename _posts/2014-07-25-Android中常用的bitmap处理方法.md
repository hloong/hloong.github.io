---
layout: mypost
title: Android中常用的bitmap处理方法
categories: [Android]
---
收集了很多bitmap相关的处理方法，几乎全部应用在项目中，所以特记录下！

```

import java.io.ByteArrayOutputStream;
import java.io.IOException;

import android.content.Context;
import android.content.res.Resources;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.graphics.Canvas;
import android.graphics.Matrix;
import android.graphics.Paint;
import android.graphics.PixelFormat;
import android.graphics.PorterDuffXfermode;
import android.graphics.Rect;
import android.graphics.RectF;
import android.graphics.Bitmap.Config;
import android.graphics.PorterDuff.Mode;
import android.graphics.drawable.BitmapDrawable;
import android.graphics.drawable.Drawable;
import android.view.View;
import android.view.View.MeasureSpec;

public class ImageUtils {

	//--->bitmap相关
	//参考网站http://www.cnblogs.com/fighter/archive/2012/02/20/android-bitmap-drawable.html
	// 见博客：http://blog.sina.com.cn/s/blog_afb547c60101j7qn.html
	/**
	 * View转成bitmap
	 * @param view
	 * @return
	 */
	public static Bitmap convertViewToBitmap(View view) {
		view.setDrawingCacheEnabled(true);
		view.measure(MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED),
				MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED));
		view.layout(0, 0, view.getMeasuredWidth(), view.getMeasuredHeight());
		view.buildDrawingCache();
		return view.getDrawingCache();
	}
	/**
	 * 缩放Drawable
	 * @param drawable
	 * @param w  缩放后需要的宽度
	 * @param h  缩放后需要的高度
	 * @return
	 */
	public static Drawable zoomDrawable(Drawable drawable, int w, int h) {
        int width = drawable.getIntrinsicWidth();
        int height = drawable.getIntrinsicHeight();
        // drawable转换成bitmap
        Bitmap oldbmp = drawableToBitmap(drawable);
        // 创建操作图片用的Matrix对象
        Matrix matrix = new Matrix();
        // 计算缩放比例
        float sx = ((float) w / width);
        float sy = ((float) h / height);
        // 设置缩放比例
        matrix.postScale(sx, sy);
        // 建立新的bitmap，其内容是对原bitmap的缩放后的图
        Bitmap newbmp = Bitmap.createBitmap(oldbmp, 0, 0, width, height,
                matrix, true);
        return new BitmapDrawable(newbmp);
    }
	
	/**
	 * 缩放bitmap
	 * @param oldBitmap 输入bitmap
	 * @param newWidth 
	 * @param newHeight
	 * @return
	 */
	public static Bitmap zoomBitmap(Bitmap oldBitmap, int newWidth, int newHeight) {
		// 获得图片的宽高
		int width = oldBitmap.getWidth();
		int height = oldBitmap.getHeight();
		// 计算缩放比例
		float scaleWidth = ((float) newWidth) / width;
		float scaleHeight = ((float) newHeight) / height;
		// 取得想要缩放的matrix参数
		Matrix matrix = new Matrix();
		matrix.postScale(scaleWidth, scaleHeight);
		// 得到新的图片
		Bitmap newbm = Bitmap.createBitmap(oldBitmap, 0, 0, width, height, matrix,
				true);
		return newbm;
	}
	/**
	 * 缩放网络图片 依赖于zoomBitmap
	 * @param img
	 * @param newWidth
	 * @param newHeight
	 * @return
	 */
	public static Bitmap zoomImg(String img, int newWidth, int newHeight) {
		// 图片源
		Bitmap bm = BitmapFactory.decodeFile(img);
		if (null != bm) {
			return zoomBitmap(bm, newWidth, newHeight);
		}
		return null;
	}
	/**
	 * 缩放网络图片 依赖于zoomBitmap
	 * @param context
	 * @param img
	 * @param newWidth
	 * @param newHeight
	 * @return
	 */
	public static Bitmap zoomImg(Context context, String img, int newWidth,
			int newHeight) {
		// 图片源
		try {
			Bitmap bm = BitmapFactory.decodeStream(context.getAssets()
					.open(img));
			if (null != bm) {
				return zoomBitmap(bm, newWidth, newHeight);
			}
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return null;
	}
	/**
	 * 判断bitmap是否存在
	 * @param bitmap
	 * @return
	 */
	public static boolean bitmapAvailable(Bitmap bitmap) {
		return bitmap != null && bitmap.getWidth() > 0 && bitmap.getHeight() > 0;
	}
	/**
	 * drawable 转成bitmap
	 * @param drawable
	 * @return
	 */
	public static Bitmap drawableToBitmap(Drawable drawable) {
        // 取 drawable 的长宽
        int w = drawable.getIntrinsicWidth();
        int h = drawable.getIntrinsicHeight();
        // 取 drawable 的颜色格式
        Bitmap.Config config = drawable.getOpacity() != PixelFormat.OPAQUE ? Bitmap.Config.ARGB_8888
                : Bitmap.Config.RGB_565;
        // 建立对应 bitmap
        Bitmap bitmap = Bitmap.createBitmap(w, h, config);
        // 建立对应 bitmap 的画布
        Canvas canvas = new Canvas(bitmap);
        drawable.setBounds(0, 0, w, h);
        // 把 drawable 内容画到画布中
        drawable.draw(canvas);
        return bitmap;
    }
	/**
	 * Bitmap转换成Drawable
	 * @param context
	 * @param bitmap
	 * @return
	 */
	public static Drawable bitmapToDrawable(Context context,Bitmap bitmap){
		//因为BtimapDrawable是Drawable的子类，最终直接使用bd对象即可。
		BitmapDrawable bd= new BitmapDrawable(context.getResources(), bitmap);
		return bd;
	}
	
	/**
	 * 从资源中获取Bitmap
	 * @param context
	 * @param req  R.drawable.icon(eg.)
	 * @return
	 */
	public Bitmap getBitmapFromResources(Context context,int req){
		  Resources res = context.getResources();
		  Bitmap bmp = BitmapFactory.decodeResource(res, req);
		  return bmp;
	}
	
	/**
	 * Byte[] -> Bitmap的转换
	 */
	public Bitmap Bytes2Bimap(byte[] b) {
        if (b.length != 0) {
            return BitmapFactory.decodeByteArray(b, 0, b.length);
        } else {
            return null;
        }
    }
	/**
	 * Bitmap->Byte[]的转换
	 * @param bm
	 * @return
	 */
	 public byte[] Bitmap2Bytes(Bitmap bm) {
		 ByteArrayOutputStream baos = new ByteArrayOutputStream();
		 bm.compress(Bitmap.CompressFormat.PNG, 100, baos);
		 return baos.toByteArray();
    }
	/**
	 * 获取圆角图片
	 * @param bitmap
	 * @param roundPx 圆角的弧度
	 * @return
	 */
	public static Bitmap getRoundedCornerBitmap(Bitmap bitmap, float roundPx) {
        int w = bitmap.getWidth();
        int h = bitmap.getHeight();
        Bitmap output = Bitmap.createBitmap(w, h, Config.ARGB_8888);
        Canvas canvas = new Canvas(output);
        final int color = 0xff424242;
        final Paint paint = new Paint();
        final Rect rect = new Rect(0, 0, w, h);
        final RectF rectF = new RectF(rect);
        paint.setAntiAlias(true);
        canvas.drawARGB(0, 0, 0, 0);
        paint.setColor(color);
        canvas.drawRoundRect(rectF, roundPx, roundPx, paint);
        paint.setXfermode(new PorterDuffXfermode(Mode.SRC_IN));
        canvas.drawBitmap(bitmap, rect, rect, paint);
        return output;
    }
}
```