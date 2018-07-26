## Android 代码规范

### 起因

五湖四海的小伙伴聚在一起，每个人的开发习惯和代码规范都不同，有的人代码整洁，变量命名和格式看着都很舒心，有的小伙伴提交代码的时候就没对代码进行格式化，或者不同类型的代码放的位置不同，出现魔法数字等，让人不想继续看下去。

因为团队业务需要，参考了《阿里巴巴java开发手册》和《阿里巴巴Android手册》，总结出一份简单版本，可供小伙伴们快速统一规范

### 目录

1. 命名风格
2. 代码格式
3. Android 基本组件
4. 集合相关
5. 动画和图片



### 命名风格

1. 方法名、参数名、成员变量、局部变量都统一使用 lowerCamelCase 风格，必须遵从 驼峰形式 。

   ```
   正例: localValue / getHttpMessage() / inputUserId
   ```

2. 常量命名全部大写，不要单独在代码中存在 “0”， “1” 类似的魔法数字，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长，**并且常量要放到类文件的最上方** 。

   ```
   正例: public static final int REQUEST_KEY = 1;
   ```

3. **包名统一使用小写**，点分隔符之间有且仅有一个自然语义的英语单词。**包名统一使用 单数形式** ，例如 `util`包，不要使用 `utils`，但是类名如果有复数含义，类名可以使用复数形式。  

   ```
   正例:应用工具类包名为 com.alibaba.ai.util、类名为 MessageUtils(此规则参考 spring 的框架结构)
   ```

4. View 控件命名:以驼峰法命名，View 组件的资源 id 需要以 View 的缩写作为 前缀。常用缩写表如下: 

   | 控件             | 缩写 |
   | ---------------- | :--- |
   | LinearLayout     | ll   |
   | RelativeLayout   | rl   |
   | ConstraintLayout | cl   |
   | ListView         | lv   |
   | ScollView        | sv   |
   | TextView         | tv   |
   | Button           | btn  |
   | ImageView        | iv   |
   | CheckBox         | cb   |
   | RadioButton      | rb   |
   | EditText         | et   |

   在 xml 文件里可以这样定义，下划线分割：

   ```
   <TextView
                   android:id="@+id/tv_stage_name"
                   android:layout_width="wrap_content"
                   android:layout_height="match_parent"
                   android:layout_marginLeft="15dp"
                   android:gravity="center_vertical"
                   android:text="@string/craftsman_name"
                   android:textColor="@color/color_888888"
                   android:textSize="15sp" />
   ```

   在 Activity 中可以这样定义，驼峰命名：

   ```
   TextView tvStageName;
   ```

5. 颜色命名，颜色名字母同一用小写，例如:

   ```
   <color name="color_ff9862">#ff9862</color>
   ```

6. xml 文件命名，如果是在 app 下的，不用加 module 名，否则文件最开始要加上 moudle_

   背景命名：solid 颜色 + strock 颜色 + strock 宽度(默认 0.5dp)，例如：

   ```
   <shape xmlns:android="http://schemas.android.com/apk/res/android">
   
       <solid android:color="@color/color_fafafa" />
   
       <stroke
           android:width="0.5dp"
           android:color="@color/color_fafafa" />
   </shape>
   ```

   可以写成：`s_fafafa_r_fafafa_1`



### 代码格式

1. 代码编写完提交前，一定要统一用 AndroidStudio 进行代码格式化后，再提交

2. 不同类型代码的位置,以下面代码为例

   + 常量放在类最开始的位置，并且每个常量尽量有注释说明意思
   + 然后放 Butterknife 注解的控件 View，每一个部分的控件建议分为一组，并加上注释
   + 然后放类中使用的成员变量，也建议分组，中间加上空行

   ```
   public class YJAuthorisationActivity extends BaseActivity {
   
       /**
        * Intent 的 KEY
        */
       public static final String AUTHORIZATION_KEY_SUCCESS = "SUCCESS";
   
       public static final int AUTHORIZATION_PASS = 3;      //已认证
       public static final int AUTHORIZATION_FAIL = 2;      //未通过
       public static final int AUTHORIZATION_IN_PROCESS = 1;//审核中
       public static final int AUTHORIZATION_NONE = 0;      //未认证
   
       public static final String FRONT = "Front"; //正面照
       public static final String BACK = "Back";        //  反面照
       public static final String HAND = "Hand";    //手持身份证照
   
       private static final int GALLERY_REQUEST_CODE = 0;  // 相册选图标记
       private static final int CAMERA_REQUEST_CODE = 1;   // 相机拍照标记
   
       // 标题栏
       @Bind(R.id.iv_back)
       ImageView ivBack;
       // 分页布局
       @Bind(R.id.ll_auth_base_info)
       LinearLayout llAuthBaseInfo;
       @Bind(R.id.sc_auth_card_info)
       ScrollView scAuthCardInfo;
       @Bind(R.id.ll_auth_result)
       LinearLayout llAuthResult;
       // 认证结果页面
       @Bind(R.id.tv_stage_name_result)
       TextView tvStageNameResult;
       @Bind(R.id.tv_real_name_result)
       TextView tvRealNameResult;
       @Bind(R.id.tv_phone_num_result)
       TextView tvPhoneNumResult;
   
       /**
        * 错误位置
        */
       private boolean stageNameError;
       private boolean realNameError;
       private boolean phoneNumError;
       private boolean cardNumError;
   
       /**
        * 是否需要显示键盘
        */
       private boolean stageNameShow;
       private boolean realNameShow;
       private boolean cardNumShow;
       
       // 认证费用
       private int approveFee;
       private int approveRebate;
   
       private AuthManager mAuthManager;
   
       @Override
       protected void onCreate(@Nullable Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
   
           mAuthManager = new AuthManager(this);
           mAuthManager.init(scAuthCardInfo, llBaseInfo);
   
           initView();
           initData();
       }
   
       @Override
       protected void onNewIntent(Intent intent) {
           super.onNewIntent(intent);
   
           requestAuthorizationDetail();
       }
   
       /**
        * 初始化视图相关
        */
       private void initView() {
           ivBack.setOnClickListener(new View.OnClickListener() {
               @Override
               public void onClick(View v) {
                   mAuthManager.saveSharedPreferences(etStageName, etRealName, tvPhoneNum, etCardNumber);
                   finish();
               }
           });
           etStageName.setOnClickListener(new View.OnClickListener() {
               @Override
               public void onClick(View v) {
                   etStageName.setFocusableInTouchMode(true);
                   etStageName.requestFocus();
                   if (stageNameShow) {
                       stageNameShow = false;
                       KeyboardUtil.softShow(mContext);
                   }
               }
           });
       }
   } 
   ```

3. 避免在系统的方法里，例如 `onCreate()` 等写太多行代码，例如可以定义两个方法，分别进行初始化视图操作和初始化对象初始化操作：

   ```
   initView();
   initData();
   ```

4. 每个方法的长度尽量不要超过100行，多的代码可以通过增加方法来拆分

5. 方法如果不能见名知意，需要对方法加上注释

6. 注释的 `//` 后面需要加上一个空格

   ```
   // 认证成功
   ```

7. 跳转 Activity ，如果需要 Intent 传值，传值的 KEY 必须在目标 Activiy 中声明常量，例如:

   ```
   Intent intent = new Intent(mContext, YJAuthorisationActivity.class);
   // 注意 key 是常量类型
   intent.putExtra(YJAuthorisationActivity.AUTHORIZATION_KEY_SUCCESS, true);
   mContext.startActivity(intent);
   ```



### Android 基本组件  

1. 不要在 Android 的 Application 对象中缓存数据。基础组件之间的数据共享 请使用 Intent 等机制，也可使用 SharedPreferences 等数据持久化机制。 

   即不要在 Application 里使用 public static 类型的变量来保存数据

2. Activity 间的数据通信，对于数据量比较大的，避免使用 Intent + Parcelable 的方式，可以考虑 EventBus 等替代方案，以免造成 TransactionTooLargeException。 

3. Activity 间通过 **隐式 Intent 的跳转**，在发出 Intent 之前必须通过 resolveActivity 检查，避免找不到合适的调用组件，造成 ActivityNotFoundException 的异常。 

4. 当前 Activity 的 onPause 方法执行结束后才会执行下一个 Activity 的 onCreate 方法，所以在 onPause 方法中不适合做耗时较长的工作，这会影响到页面之间的跳 转效率。 

5. 避免在 `Service#onStartCommand()/onBind() ` 方法中执行耗时操作，如果确 实有需求，应改用 IntentService 或采用其他异步机制完成。 

6. 避免在 BroadcastReceiver#onReceive()中执行耗时操作，如果有耗时工作， 应该创建 IntentService 完成，而不应该在 BroadcastReceiver 内创建子线程去做 。

7. 不要在 `Activity#onDestroy()` 内执行释放资源的工作，例如一些工作线程的 销毁和停止，因为 `onDestroy()` 执行的时机可能较晚。可根据实际需要，在 `Activity#onPause()/onStop()` 中结合 `isFinishing()` 的判断来执行。 

8. 添 加 Fragment 时 ， 确 保 FragmentTransaction#commit() 在 Activity#onPostResume()或者 FragmentActivity#onResumeFragments()内调用。 不要随意使用 FragmentTransaction#commitAllowingStateLoss()来代替，任何 commitAllowingStateLoss()的使用必须经过 code review，确保无负面影响。 

   说明: 

   Activity 可能因为各种原因被销毁，Android 支持页面被销毁前通过 Activity#onSaveInstanceState() 保 存 自 己 的 状 态 。 但 如 果 

   FragmentTransaction.commit()发生在 Activity 状态保存之后，就会导致 Activity 重 建、恢复状态时无法还原页面状态，从而可能出错。为了避免给用户造成不好的体 

   验，系统会抛出 IllegalStateExceptionStateLoss 异常。推荐的做法是在 Activity 的 

   onPostResume() 或 onResumeFragments() ( 对 FragmentActivity ) 里 执 行 FragmentTransaction.commit()，如有必要也可在 onCreate()里执行。不要随意改用 

   FragmentTransaction.commitAllowingStateLoss()或者直接使用 try-catch 避免 crash，这不是问题的根本解决之道，当且仅当你确认 Activity 重建、恢复状态时， 

   本次 commit 丢失不会造成影响时才可这么做。 

### 集合相关

1. 使用集合转数组的方法，必须使用集合的toArray(T[] array)，传入的是类型完全 一样的数组，大小就是 list.size()。 

   说明:使用 toArray 带参方法，入参分配的数组空间不够大时，toArray 方法内部将重新分配 

   内存空间，并返回新数组地址;如果数组元素个数大于实际所需，下标为[ list.size() ] 

   的数组元素将被置为 null，其它数组元素保持原值，因此最好将方法入参数组大小定义与集 

   合元素个数一致。 

   ```
   正例:
   List<String> list = new ArrayList<String>(2); list.add("guan");
   list.add("bao");
   String[] array = new String[list.size()]; array = list.toArray(array);
   ```

2. 使用工具类 Arrays.asList()把数组转换成集合时，不能使用其修改集合相关的方 法，它的 add/remove/clear 方法会抛出 UnsupportedOperationException 异常。 说明:asList 的返回对象是一个 Arrays 内部类，并没有实现集合的修改方法。Arrays.asList 体现的是适配器模式，只是转换接口，后台的数据仍是数组。 

   ```
   String[] str = new String[] { "you", "wu" };
   List list = Arrays.asList(str); 
   
   ```

   第一种情况:`list.add("yangguanbao");` 运行时异常。

    第二种情况:`str[0] = "gujin";` 那么list.get(0)也会随之修改。

3. 不要在 foreach 循环里进行元素的 remove/add 操作。remove 元素请使用 Iterator 

   方式，如果并发操作，需要对 Iterator 对象加锁。 

   ```
   正例:
   List<String> list = new ArrayList<>(); list.add("1");
   list.add("2");
   Iterator<String> iterator = list.iterator(); while (iterator.hasNext()) {
   String item = iterator.next(); if (删除元素的条件) {
                  iterator.remove();
              }
    }
    
    
    反例:
   for (String item : list) { if ("1".equals(item)) {
                  list.remove(item);
              }
   }
   ```



### 动画和图片

1. 使用同一的图片加载库：目前使用 Glide
2. 使用完毕的图片，应该及时回收，释放宝贵的内存。 BItmap 用完以后要及时的 `recycle() `
3. 在 Activity.onPause()或 Activity.onStop()回调中，关闭当前 activity 正在执行的的动画。 