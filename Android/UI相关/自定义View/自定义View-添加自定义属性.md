### 添加属性

1. 在 `values` 文件夹下面创建 `attr_xxx.xml` 文件

2. 定义属性, 加上一个 name 如下：

   ```
   <resources>
   
       <!-- Base application theme. -->
       <declare-styleable name="ShapeView">
           <attr name="type" format="enum">
               <enum name="cycle" value="0" />
               <enum name="rect" value="1" />
               <enum name="arc" value="2" />
               <enum name="text" value="3" />
               <enum name="line" value="4" />
           </attr>
       </declare-styleable>
   
   </resources>
   ```

3. 代码里通过 `TypedArray` 获取属性：

   ```
       private int type;
   
       public ShapeView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
           super(context, attrs, defStyleAttr);
   
           TypedArray ta = context.obtainStyledAttributes(attrs, R.styleable.ShapeView);
           type = ta.getInteger(R.styleable.ShapeView_type, 0);
           ta.recycle();
       }
   ```



### 属性类型

1. `enum` 类型

   ```
   <attr name="type" format="enum">
   	<enum name="cycle" value="0" />
   	<enum name="rect" value="1" />
   </attr>
   ```

   如上面demo所示，好处是在写布局的时候，定义的属性可以有一个英文名字

2. `int`

   ```
   <attr name="intType" format="integer"/>
   ```

3. bool

   ```
   <attr name="boolType" format="boolean"/>
   ```
