## Junit 使用

只要记住三个注解就可以了：

1. @Before ：进行一些初始化的操作
2. @Test：要执行的测试用例代码，使用 Assert 断言来判断是否通过用例
3. @After：进行一些资源回收的操作

```
public class ShiroDemoApplicationTests {

    @Before
    public void init() {
        System.out.println("init");
    }

    @After
    public void close() {
        System.out.println("close");
    }

    @Test
    public void contextLoads() {
        System.out.println("okk");

        // 使用断言来判断结果
        Assert.assertEquals(1,2);
    }

}
```

