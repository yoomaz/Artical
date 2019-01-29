## Java 动态代理

  #### 什么是代理

代理就是代替别人做一些事情，例如 A类 委托 B类 去做一件事情，B就是代理类，A就是委托类

 1. 静态代理

    可以看下下面的代码：

    ```
    class ClassA {
        public void doWork1() {};
    
        public void doWork2() {};
    }
    
    public class ClassB {
        private ClassA a;
    
        public ClassB(ClassA a) {
            this.a = a;
        }
    
        public void operateMethod1() {
            a.doWork1();
        };
    
        public void operateMethod2() {
            a.doWork2();
        };
    }
    ```

    如果我们需要 A 去实现一些功能，这里不需要直接掉 A 的方法，直接调用 B 的方法即可，B 对 A 的方法做了一层封装。这里A 是委托类， B 是代理类

    这种在程序运行前已经存在的代理类称为静态代理。

    缺点：B 类需要事先写好，如果 A 类的方法名称变化了，也需要修改 B类。

	2. 动态代理

    相对于静态代理，动态代理的代理类是运行的时候生成的，即 B 类不是我们自己去实现的，用 Java 的动态代理机制生成的。

#### 动态代理的好处

我们可以更方便的对方法进行一些统一的处理，例如记录方法执行的时间，在函数执行前进行判断，对函数的结果进行修改等，不用像静态代理那样去修改每个方法。

#### 如何实现一个简单的动态代理

Java 的动态代理有两个关键的类需要了解：Proxy(类) 和 InvocationHandler(接口)。

Proxy 的作用是创建一个动态代理类，用的最多的是 newProxyInstance 方法：

```
public static Object newProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler h)
```

这里接受三个参数：

loader：一个ClassLoader对象，定义了由哪个ClassLoader对象来对生成的代理对象进行加载

interfaces：一个Interface对象的数组，表示的是我要给我需要代理的对象提供一组接口，生成的代理对象也会实现了该接口，这样我就能通过强转调用这组接口中的方法了

 h：一个 InvocationHandler 对象，生成的动态代理方法执行的时候，会去交给这个对象去执行。



下面实现一个简单的例子，用于统计每个方法的执行时间并打印出来：

先实现一个接口和他的实现类:

```java
public interface IWork {

    int doWork1();

    void doWork2();

}

public class IWorkImpl implements IWork {

    @Override
    public int doWork1() {
        System.out.println("doWork1");

        sleep(110);

        return 1;
    }

    @Override
    public void doWork2() {
        System.out.println("doWork2");

        sleep(120);
    }

    private static void sleep(long millSeconds) {
        try {
            Thread.sleep(millSeconds);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

然后实现一个 TimingIWorkHandler 实现 InvocationHandler 接口

```java
public class TimingIWorkHandler implements InvocationHandler {

    private Object target;

    public TimingIWorkHandler() {
    }

    public TimingIWorkHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        long start = System.currentTimeMillis();
        Object methodResult = method.invoke(target, args);

        System.out.println(method.getName() + " cost time is:" + (System.currentTimeMillis() - start));

        if (methodResult != null) {
            methodResult = (int) methodResult + 1;
        }
        return methodResult;
    }
}
```

当代理的每个方法执行的时候，会调用 TimingIWorkHandler 的 invoke 方法，真正执行方法是在`method.invoke(target, args)` ，我们在方法执行的前后记录了时间，相减记录了执行时间并打印出来。

我们注意到 `doWork1()` 是会返回一个 int 类型的变量的，我们在代理里会把结果再加一，然后返回, 实现了对方法结果的修改。如果方法是 void 的，返回的是一个 null 对象。

```java
if (methodResult != null) {
   methodResult = (int) methodResult + 1;
}
```

下面是 main() 方法:

```
public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException {
        TimingIWorkHandler timingInvocationHandler = new TimingIWorkHandler(new IWorkImpl());

        IWork operate = (IWork) (Proxy.newProxyInstance(
                IWork.class.getClassLoader(),
                new Class[]{IWork.class},
                timingInvocationHandler));

        int result = operate.doWork1();
        System.out.println("doWork1 result:" + result);

        operate.doWork2();
}
```

通过 `Proxy.newProxyInstance` 实例化了一个 IWork 代理对象。运行结果如下

```
doWork1
doWork1 cost time is:115
doWork1 result:2
doWork2
doWork2 cost time is:123
```



#### 动态代理源码分析

小 tip : 默认在 Idea 里是看不到运行过程中生成的类的，可以在 main 方法里加入下面代码进行保存:

```
Field field = System.class.getDeclaredField("props");    
field.setAccessible(true);    
Properties props = (Properties) field.get(null);    
props.put("sun.misc.ProxyGenerator.saveGeneratedFiles", "true");
```



我们先看生成的动态代理对象反编译后的源码:

```java
public final class $Proxy0 extends Proxy implements IWork {
    private static Method m1;
    private static Method m2;
    private static Method m3;
    private static Method m4;
    private static Method m0;

    public $Proxy0(InvocationHandler var1) throws  {
        super(var1);
    }

    public final boolean equals(Object var1) throws  {
        try {
            return (Boolean)super.h.invoke(this, m1, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    public final String toString() throws  {
        try {
            return (String)super.h.invoke(this, m2, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final void doWork2() throws  {
        try {
            super.h.invoke(this, m3, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final int doWork1() throws  {
        try {
            return (Integer)super.h.invoke(this, m4, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final int hashCode() throws  {
        try {
            return (Integer)super.h.invoke(this, m0, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    static {
        try {
            m1 = Class.forName("java.lang.Object").getMethod("equals", Class.forName("java.lang.Object"));
            m2 = Class.forName("java.lang.Object").getMethod("toString");
            m3 = Class.forName("dynamicproxy.IWork").getMethod("doWork2");
            m4 = Class.forName("dynamicproxy.IWork").getMethod("doWork1");
            m0 = Class.forName("java.lang.Object").getMethod("hashCode");
        } catch (NoSuchMethodException var2) {
            throw new NoSuchMethodError(var2.getMessage());
        } catch (ClassNotFoundException var3) {
            throw new NoClassDefFoundError(var3.getMessage());
        }
    }
}
```





