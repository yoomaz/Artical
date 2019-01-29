#### 一. View 动画
分为四种：平移，缩放，旋转，渐变，他们之间有很多共性，所以有一个共同的父类 Animation，存放一些动画共有的属性

1. Animation
    * `duration` 时长 
    * `fillAfter` 停止后是否留在最后一帧
    * `fillBefore` 停止后是否回到第一帧，默认为 true
    * `fillEnabled` 和 `fillBefore` 相同
    * `repeatCount` 重复的次数
    * `repeatMode` 重复的类型，分为 `reverse` 和 `restart` ，前一个表示第一次动画结束后从最后一帧往前，后一个表示第一次动画结束后再从第一帧开始，默认是 `restart`
    * `interpolator` 差值器

2. Translate 移动
    * `android:fromXDelta` 数值可以为 整数，百分数，百分数p ，例如 50、50%、50%p，
      * 50 表示以当前View左上角坐标加50px为初始点
      * 50% 表示以当前View的左上角加上当前View宽高的50%做为初始点
      * 50%p 表示以当前View的左上角加上父控件宽高的50%做为初始点，p 代表 parent
    * `android:fromYDelta`  同上
    * `android:toXDelta`  同上
    * `android:toYDelta`  同上

    ```
    <?xml version="1.0" encoding="utf-8"?>  
        <translate xmlns:android="http://schemas.android.com/apk/res/android"  
            android:fromXDelta="0"  
            android:fromYDelta="0"
            android:toXDelta="-80"  
            android:toYDelta="-80"  
            android:duration="2000">  
        </translate> 
    ```
    如果只是在 x 方向上移动，不在 y 方向上移动，只需要写成下面这样子, 不需要加上 y ：

    ```
    <?xml version="1.0" encoding="utf-8"?>  
        <translate xmlns:android="http://schemas.android.com/apk/res/android"  
            android:fromXDelta="0"  
            android:toXDelta="-80"   
            android:duration="2000">  
        </translate> 
    ```

    ​

3. Scale 缩放放大

    * `android:fromXScale` 浮点值，开始的时候 x方向 缩放或者放大比例
    * `android:toXScale` 浮点值，结束的时候 x方向 缩放或者放大比例
    * `android:fromYScale` 同上x
    * `android:toYScale` 同上x
    * `android:pivotX` 轴点 X 坐标，支持三种表示方式(数值，百分比，父 View 百分比)
    * `android:pivotY` 轴点 Y 坐标

4. Rotate 旋转
    * `android:fromDegrees` 顺时针正值，逆时针负值
    * `android:toDegrees`
    * `android:pivotX` 轴点 X 坐标，支持三种表示方式
    * `android:pivotY` 轴点 Y 坐标

    ```
    <?xml version="1.0" encoding="utf-8"?>  
    <rotate xmlns:android="http://schemas.android.com/apk/res/android"  
        android:fromDegrees="0"  
        android:toDegrees="-650"  
        android:pivotX="50%"  
        android:pivotY="50%"  
        android:duration="3000"  
        android:fillAfter="true">  
          
    </rotate>
    ```

5. Alpha 渐变
    * `android:fromAlpha` 浮点值，0.0 - 1.0， 0为全透明，1位全不透明
    * `android:toAlpha`

    ```
    <?xml version="1.0" encoding="utf-8"?>  
    <alpha xmlns:android="http://schemas.android.com/apk/res/android"  
        android:fromAlpha="1.0"  
        android:toAlpha="0.1"  
        android:duration="3000"  
        android:fillBefore="true">  
    </alpha> 
    ```

6. Set 动作合集
    也是直接继承自 `Animation`， 但是没有自己的属性，包裹其他动画，实现几个动画同时发生

    ```
    <?xml version="1.0" encoding="utf-8"?>  
    <set xmlns:android="http://schemas.android.com/apk/res/android"  
        android:duration="3000"  
        android:fillAfter="true">  
          
      <alpha   
        android:fromAlpha="0.0"  
        android:toAlpha="1.0"/>  
        
      <scale  
        android:fromXScale="0.0"  
        android:toXScale="1.4"  
        android:fromYScale="0.0"  
        android:toYScale="1.4"  
        android:pivotX="50%"  
        android:pivotY="50%"/>  
        
      <rotate  
        android:fromDegrees="0"  
        android:toDegrees="720"  
        android:pivotX="50%"  
        android:pivotY="50%"/>  
             
    </set>
    ```

7. View 动画的使用

    ```
    Animation translateAnim = AnimationUtils.loadAnimation(MainActivity.this, R.anim.translate);
    view.startAnimation(translateAnim);
    ```

#### 二. 帧动画
> 帧动画的载体是一个 `ImageViwe`， 设置一个 `AnimationDrawable`
1. 先定义一个 animation-list 类型的 xml 文件， 名字叫 item01 ，放在 drawable 文件夹下面

```
<animation-list xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:drawable="@drawable/anim_fram_08"
        android:duration="80" />
    <item
        android:drawable="@drawable/anim_fram_09"
        android:duration="80" />
    <item
        android:drawable="@drawable/anim_fram_10"
        android:duration="80" />

</animation-list>
```
2. 具体设置代码：

```
ivAnim.setImageResource(R.drawable.item01);
AnimationDrawable animationDrawable = (AnimationDrawable) ivAnim.getDrawable();
animationDrawable.start();
```

#### 三. 属性动画
> 属性动画原理是通过反射 `setProp` 方法不断修改对象的属性，例如 view 的 x 属性值等，达到动画的效果。

> 对于属性值，只设置一个的时候，会认为当前对象该属性的值为开始（getPropName反射获取），然后设置的值为终点。如果设置两个，则一个为开始、一个为结束

> 动画更新的过程中，会不断调用setPropName更新元素的属性，所有使用ObjectAnimator更新某个属性，必须得有getter（设置一个属性值的时候）和setter方法

1. ObjectAnimator
  对象动画类，在构造的过程中需要传递一个对象，主要通过 `of 方法` 改变对象的属性，因为是 String 类型的参数传入，所以没有代码提示，可以通过 `view.getXX` 方法获得具体的属性名字和属性的数据类型，第三个参数可以设置多个参数值，标识在这几个参数之间变化, 例如下面就是先从 0 到 360，再从 360 到 120。
```java
ObjectAnimator.ofFloat(view, "translationX", 0f, 360f，120f)
                        .setDuration(500)
                        .start();
```

Animator 有 `addUpdateListener()` 方法，用来设置在动画执行过程中的回调监听。

例如下面，随便设置了一个属性，因为属性值是逐渐变化的，变化的过程中会有触发回调，我们在回调里设置了`scale 和 alpha 属性`：

```
ObjectAnimator animator = ObjectAnimator.ofFloat(view, "valuae", 1.0f, 0f, 1.0f)
                        .setDuration(1000);
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
                    @Override
                    public void onAnimationUpdate(ValueAnimator animation) {
                        float animatValue = (float) animation.getAnimatedValue();
                        Log.i("value", animatValue + "");
                        view.setScaleX(animatValue);
                        view.setScaleY(animatValue);
                        view.setAlpha(animatValue);
                    }
                });
                animator.start();
```

`PropertyValuesHolder` 属性持有器
用来保存一些属性的变化过程，构造的时候只有属性和变化的数值，方便组合动画

```
PropertyValuesHolder scaleXHolder = PropertyValuesHolder.ofFloat("scaleX", 1.0f, 0f, 1.0f);
PropertyValuesHolder scaleYHolder = PropertyValuesHolder.ofFloat("scaleY", 1.0f, 0f, 1.0f);
PropertyValuesHolder alphaHolder = PropertyValuesHolder.ofFloat("alpha", 1.0f, 0f, 1.0f);
ObjectAnimator.ofPropertyValuesHolder(view, scaleXHolder, scaleYHolder, alphaHolder)
                        .setDuration(1000)
                        .start();
```

2. ValueAnimator
  构造的时候只传入变化的数值，然后在 `animator.addUpdateListener` 中根据动画的数值手动进行动画

```
ValueAnimator valueAnimator = ValueAnimator.ofFloat(1.0f, 0f, 1.0f);
valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
                    @Override
                    public void onAnimationUpdate(ValueAnimator animation) {
                        float value = (float) animation.getAnimatedValue();
                        view.setScaleX(value);
                        view.setScaleY(value);
                        view.setAlpha(value);
                        view.setPivotX(0);
                        view.setPivotY(0);
                    }
                });
valueAnimator.start();
```