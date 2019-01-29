1. Activity -> ViewGroup -> View 事件传递

   1. view 为 clickable

      ```
      09-24 10:19:03.125 27649-27649/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_DOWN
      09-24 10:19:03.126 27649-27649/com.yooma.demo_customview I/DemoViewGroup: dispatchTouchEvent: ACTION_DOWN
          onInterceptTouchEvent: ACTION_DOWN
      09-24 10:19:03.126 27649-27649/com.yooma.demo_customview I/DemoView: dispatchTouchEvent: ACTION_DOWN
      09-24 10:19:03.127 27649-27649/com.yooma.demo_customview I/DemoView: onTouchEvent: ACTION_DOWN
      09-24 10:19:03.163 27649-27649/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_MOVE
      09-24 10:19:03.164 27649-27649/com.yooma.demo_customview I/DemoViewGroup: dispatchTouchEvent: ACTION_MOVE
          onInterceptTouchEvent: ACTION_MOVE
      09-24 10:19:03.164 27649-27649/com.yooma.demo_customview I/DemoView: dispatchTouchEvent: ACTION_MOVE
      09-24 10:19:03.165 27649-27649/com.yooma.demo_customview I/DemoView: onTouchEvent: ACTION_MOVE
      09-24 10:19:03.167 27649-27649/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_MOVE
      09-24 10:19:03.167 27649-27649/com.yooma.demo_customview I/DemoViewGroup: dispatchTouchEvent: ACTION_MOVE
          onInterceptTouchEvent: ACTION_MOVE
      09-24 10:19:03.167 27649-27649/com.yooma.demo_customview I/DemoView: dispatchTouchEvent: ACTION_MOVE
      09-24 10:19:03.168 27649-27649/com.yooma.demo_customview I/DemoView: onTouchEvent: ACTION_MOVE
      09-24 10:19:03.170 27649-27649/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_UP
      09-24 10:19:03.171 27649-27649/com.yooma.demo_customview I/DemoViewGroup: dispatchTouchEvent: ACTION_UP
          onInterceptTouchEvent: ACTION_UP
      09-24 10:19:03.171 27649-27649/com.yooma.demo_customview I/DemoView: dispatchTouchEvent: ACTION_UP
          onTouchEvent: ACTION_UP
      ```

   2. view 不是 clickable 的

      ```
      09-24 10:21:37.838 27970-27970/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_DOWN
      09-24 10:21:37.839 27970-27970/com.yooma.demo_customview I/DemoViewGroup: dispatchTouchEvent: ACTION_DOWN
          onInterceptTouchEvent: ACTION_DOWN
      09-24 10:21:37.839 27970-27970/com.yooma.demo_customview I/DemoView: dispatchTouchEvent: ACTION_DOWN
      09-24 10:21:37.840 27970-27970/com.yooma.demo_customview I/DemoView: onTouchEvent: ACTION_DOWN
      09-24 10:21:37.840 27970-27970/com.yooma.demo_customview I/DemoViewGroup: onTouchEvent: ACTION_DOWN
      09-24 10:21:37.842 27970-27970/com.yooma.demo_customview I/MainActivity: onTouchEvent: ACTION_DOWN
      09-24 10:21:37.865 27970-27970/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_MOVE
          onTouchEvent: ACTION_MOVE
      09-24 10:21:37.883 27970-27970/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_MOVE
      09-24 10:21:37.884 27970-27970/com.yooma.demo_customview I/MainActivity: onTouchEvent: ACTION_MOVE
      09-24 10:21:37.894 27970-27970/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_MOVE
          onTouchEvent: ACTION_MOVE
      09-24 10:21:37.896 27970-27970/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_UP
          onTouchEvent: ACTION_UP
      ```

   3. ViewGroup 把所有事件都拦截，但是在 onTouchEvent 里不消耗

      最终事件还是交给 Activity 里去处理，而且后续事件不会再发给 ViewGroup

      ```
      09-24 10:23:03.417 28206-28206/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_DOWN
          
          --------- beginning of system
      09-24 10:23:03.419 28206-28206/com.yooma.demo_customview I/DemoViewGroup: dispatchTouchEvent: ACTION_DOWN
      09-24 10:23:03.420 28206-28206/com.yooma.demo_customview I/DemoViewGroup: onInterceptTouchEvent: ACTION_DOWN
      09-24 10:23:03.421 28206-28206/com.yooma.demo_customview I/DemoViewGroup: onTouchEvent: ACTION_DOWN
      09-24 10:23:03.422 28206-28206/com.yooma.demo_customview I/MainActivity: onTouchEvent: ACTION_DOWN
      09-24 10:23:03.498 28206-28206/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_MOVE
          onTouchEvent: ACTION_MOVE
      09-24 10:23:03.500 28206-28206/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_UP
          onTouchEvent: ACTION_UP
      ```

   4. ViewGroup 只拦截 Move 事件

      ```
      09-24 10:29:56.911 29772-29772/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_DOWN
      09-24 10:29:56.912 29772-29772/com.yooma.demo_customview I/DemoViewGroup: dispatchTouchEvent: ACTION_DOWN
          onInterceptTouchEvent: ACTION_DOWN
      09-24 10:29:56.912 29772-29772/com.yooma.demo_customview I/DemoView: dispatchTouchEvent: ACTION_DOWN
      09-24 10:29:56.913 29772-29772/com.yooma.demo_customview I/DemoView: onTouchEvent: ACTION_DOWN
      09-24 10:29:56.913 29772-29772/com.yooma.demo_customview I/DemoViewGroup: onTouchEvent: ACTION_DOWN
      09-24 10:29:56.914 29772-29772/com.yooma.demo_customview I/MainActivity: onTouchEvent: ACTION_DOWN
      09-24 10:29:56.977 29772-29772/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_MOVE
      09-24 10:29:56.978 29772-29772/com.yooma.demo_customview I/MainActivity: onTouchEvent: ACTION_MOVE
      09-24 10:29:56.981 29772-29772/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_MOVE
          onTouchEvent: ACTION_MOVE
      09-24 10:29:56.983 29772-29772/com.yooma.demo_customview I/MainActivity: dispatchTouchEvent: ACTION_UP
      09-24 10:29:56.984 29772-29772/com.yooma.demo_customview I/MainActivity: onTouchEvent: ACTION_UP
      ```
