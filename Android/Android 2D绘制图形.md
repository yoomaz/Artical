## 矩形    
关键元素：**左上坐标** 和 **右下坐标**
```java
    private void drawRec(Canvas canvas) { 
        Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);
        paint.setColor(Color.RED); 
        float left = 0; float top = 0; 
        float right = getWidth() / 3; 
        float buttom = getWidth() / 3; 
        canvas.drawRect(left, top, right, buttom, paint);
    }
```

## 圆形
关键元素：**圆心坐标** 和 **半径**
```
    private void drawCicle(Canvas canvas) { 
        Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG); 
        paint.setColor(Color.RED); 
        float cenetrX = getWidth() / 2; 
        float centerY = getWidth() / 3 / 2; 
        float radius = getWidth() / 3 / 2;   
        canvas.drawCircle(cenetrX, centerY, radius, paint);
    }
```

## 椭圆形或者弧形 
关键元素：**椭圆容器** ， **起始角度** ， **圆弧角度**
注意：这里需要传递一个 RectF 作为椭圆或者弧形的容器        
startAngle ： 0度为最右侧        
useCenter  ： 是否是实心的

```java
    private void drawArc(Canvas canvas) {
        Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);
        paint.setColor(Color.RED);paint.setStrokeWidth(0.1f * getWzidth() / 3);
        paint.setStyle(Paint.Style.STROKE);float margin = 0.1f * getWidth() / 3;
        float left = 2 * getWidth() / 3 + margin;
        float top = 0 + margin;
        float right = getWidth() - margin;
        float buttom = getWidth() / 3 - margin;
        RectF oval = new RectF(left, top, right, buttom);
        float startAngle = 270;
        float sweepAngle = 285;
        boolean useCenter = false;
        canvas.drawArc(oval, startAngle, sweepAngle, useCenter, paint);
    }
```
## 文字
注意：
1.  `x，y` 为文字矩形的左下角坐标
2.  如果设置 `paint.setTextAlign(Paint.Align.CENTER);` 则 `x` 为文字中心的坐标，`y` 不变

```java
canvas.drawText("绘制文字", x, y, paint);
```