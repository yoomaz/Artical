1. @Get 基本注解

   ```
   @GET("blog/{id}") //这里的{id} 表示是一个变量
   Call<ResponseBody> getBlog(/** 这里的id表示的是上面的{id} */@Path("id") int id);
   ```

2. @Http 全名注解

   ```
   /**
    * method 表示请求的方法，区分大小写，retrofit 不会做处理
    * path表示路径
    * hasBody表示是否有请求体
    */
   @HTTP(method = "GET", path = "blog/{id}", hasBody = false)
   Call<ResponseBody> getBlog(@Path("id") int id);
   ```

3. @Post :

   ```
   /**
    * {@link FormUrlEncoded} 表明是一个表单格式的请求（Content-Type:application/x-www-form-urlencoded）
    * <code>Field("username")</code> 表示将后面的 <code>String name</code> 中name的取值作为 username 的值
    */
   @POST("/form")
   @FormUrlEncoded
   Call<ResponseBody> testFormUrlEncoded1(@Field("username") String name, @Field("age") int age);

   /**
    * Map的key作为表单的键
    */
   @POST("/form")
   @FormUrlEncoded
   Call<ResponseBody> testFormUrlEncoded2(@FieldMap Map<String, Object> map);
   ```

4. @Headers 和 @Header

   ```
   @GET("/headers?showAll=true")
   @Headers({"CustomHeader1: customHeaderValue1", "CustomHeader2: customHeaderValue2"})
   Call<ResponseBody> testHeader(@Header("CustomHeader3") String customHeaderValue3);
   ```

   @Headers ： 用在方法上面，请求的时候添加固定的 Header 值

   @Header ： 只能用在方法参数中，可以动态的传递 Header 参数

5. @Query 和 @QueryMap

   ```
   /**
    * 当GET、POST...HTTP等方法中没有设置Url时，则必须使用 {@link Url}提供
    * 对于Query和QueryMap，如果不是String（或Map的第二个泛型参数不是String）时
    * 会被默认会调用toString转换成String类型
    * Url支持的类型有 okhttp3.HttpUrl, String, java.net.URI, android.net.Uri
    * {@link retrofit2.http.QueryMap} 用法和{@link retrofit2.http.FieldMap} 用法一样，不再说明
    */
   @GET //当有URL注解时，这里的URL就省略了
   Call<ResponseBody> testUrlAndQuery(@Url String url, @Query("showAll") boolean showAll);
   ```

   当@GET、POST...HTTP等注解中没有设置Url时，则必须在方法参数中使用 @Url 来提供 Url

   @Query 和 @QueryMap 会自动当成参数来拼接 Url

6. @Body

   ```
   @POST("blog")
   Call<Result<Blog>> createBlog(@Body Blog blog);
   ```

   当参数比较多的时候，一种是使用 @FieldMap 注解，一种是使用 @Body 传递一个对象，当对象作为参数的时候，需要在 Retrofit 配置一个转换器，例如下面，这样就会自动把这个对象进行一次转换放到 request 的 body 里传过去

   ```
   Retrofit retrofit = new Retrofit.Builder()
           .baseUrl("http://localhost:4567/")
           //可以接收自定义的Gson，当然也可以不传
           .addConverterFactory(GsonConverterFactory.create(gson))
           .build();
   ```

   ​