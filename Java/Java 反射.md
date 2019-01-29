## Java 反射

java 反射是框架的核心，主要分为下面几种：

1. 获取成员变量
2. 获取构造函数
3. 获取普通方法

首先定义一个 Person 类：

```java
public class Person {

    public String publicField = "publicFieldValue";

    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getPublicField() {
        return publicField;
    }

    public void setPublicField(String publicField) {
        this.publicField = publicField;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void eat() {
        System.out.println("non");
    }

    public void eat(String value) {
        System.out.println(value);
    }

    @Override
    public String toString() {
        return "Person{" +
                "publicField='" + publicField + '\'' +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

```



#### 获取成员变量

1. 获取public 修饰的变量:  getFields()   /   getField(),  

   ```java
   // 获取所有 public 修饰的变量
   Field[] fields = clazz.getFields();
   for (Field field : fields) {
   	System.out.println(field);
   }
   
   // 获取所有 public 修饰的某个变量
   Field publicField = clazz.getField("publicField");
   System.out.println(publicField);
   
   Person person = new Person();
   String publicFieldValue = (String) publicField.get(person);
   System.out.println(publicFieldValue);
   ```

2. 获取所有 private 修饰的变量：getDeclaredFields() / getDeclaredField()

   ```java
   Field[] declaredFields = clazz.getDeclaredFields();
   for (Field field : declaredFields) {
   	System.out.println(field);
   }
   ```

3. 获取或者修改变量, 如果是 private 修饰的，需要调用 setAccessible() 获取权限

   ```java
   Field privateField = clazz.getDeclaredField("name");
   System.out.println(privateField);
   
   Person person = new Person();
   
   privateField.setAccessible(true); // get 和 set 前反射
   
   privateField.set(person, "privateValue");
   
   String privateFieldValue = (String) privateField.get(person);
   System.out.println(privateFieldValue);
   ```

#### 获取构造函数

Class.getConstructor() 方法，后面可以传入构造函数的参数类型来获取指定的构造函数，然后通过 Constructor.newInstance() 来创建一个对象。

如果是无参数的构造函数，可以直接 Class.newInstance() 方法创建一个对象

```java
Constructor<Person> constructor = clazz.getConstructor(String.class, int.class);
Person person = constructor.newInstance("name", 11);
System.out.println(person);

// 对于无参构造器，可以直接使用 Class.newInstance
Person person1 = clazz.newInstance();
System.out.println(person1);
```

#### 获取方法

对应 Method 类，Class.getMethod("methodName", "param"...) 方法

通过 invoke 方法来调用对象的方法，第一个参数是对象，后面的参数是形参

```java
Method eat = clazz.getMethod("eat");

Person person = new Person();
eat.invoke(person);

Method eat2 = clazz.getMethod("eat", String.class);
eat2.invoke(person, "banana");

// 获取方法名
System.out.println(eat.getName());
```

#### 应用

编写一个框架，在配置文件定义类名和方法，然后框架执行

1. 先在 src 下创建一个 pro.properties 配置文件，内容如下：

   ```properties
   name=com.graypn.reflect.Person
   method=eat
   ```

2. 获取配置文件的类名和方法名

   ```java
   ClassLoader classLoader = ReflectDemo01.class.getClassLoader();
   InputStream resourceAsStream = classLoader.getResourceAsStream("pro.properties");
   Properties properties = new Properties();
   properties.load(resourceAsStream);
   
   String name = properties.getProperty("name");
   String method = properties.getProperty("method");
   ```

3. 把类加载到内存，创建一个对象实例，并获取方法 invoke 执行

   ```java
   // 加载类，执行类的的方法
   Class aClass = Class.forName(name);
   Object object = aClass.newInstance();
   Method method1 = aClass.getMethod(method);
   
   method1.invoke(object);
   ```

#### 扩展：获取 Class 的三种方式

1. Class.forName()
2. Object.getClass()
3. 类名.Class

类只有加在到内存，才能获取 Class 对象，并在在内存中只存一份

在内存中没有加载类的时候，调用2后者3的方式获取 Class，会 ClassNotFound 错误



最后附上完整 Demo:

```java
package com.graypn.reflect;

import java.io.InputStream;
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;
import java.util.Properties;



public class ReflectDemo01 {

    public static void main(String args[]) throws Exception {
        Class<Person> clazz = Person.class;

        /**
         * 获取所有 public 修饰的变量
         */
        Field[] fields = clazz.getFields();
        for (Field field : fields) {
            System.out.println(field);
        }

        /**
         * 获取所有 public 修饰的某个变量
         */
        Field publicField = clazz.getField("publicField");
        System.out.println(publicField);

        Person person = new Person();
        String publicFieldValue = (String) publicField.get(person);
        System.out.println(publicFieldValue);


        /**
         * 获取所有 private 修饰的变量
         */
        Field[] declaredFields = clazz.getDeclaredFields();
        for (Field field : declaredFields) {
            System.out.println(field);
        }

        /**
         * 获取所有 public 修饰的某个变量
         */
        Field privateField = clazz.getDeclaredField("name");
        System.out.println(privateField);

        Person person = new Person();

        privateField.setAccessible(true); // get 和 set 前反射

        privateField.set(person, "privateValue");

        String privateFieldValue = (String) privateField.get(person);
        System.out.println(privateFieldValue);


        /**
         * 获取构造函数
         */
        Constructor<Person> constructor = clazz.getConstructor(String.class, int.class);
        Person person = constructor.newInstance("name", 11);
        System.out.println(person);

        // 对于无参构造器，直接使用 Class.newInstance
        Person person1 = clazz.newInstance();
        System.out.println(person1);

        /**
         * 获取方法
         */
        Method eat = clazz.getMethod("eat");

        Person person = new Person();
        eat.invoke(person);

        Method eat2 = clazz.getMethod("eat", String.class);
        eat2.invoke(person, "banana");

        // 获取方法名
        System.out.println(eat.getName());


        /**
         * 写一个简单框架，执行配置文件类的配置方法
         */
        // 读取配置文件
        ClassLoader classLoader = ReflectDemo01.class.getClassLoader();
        InputStream resourceAsStream = classLoader.getResourceAsStream("pro.properties");
        Properties properties = new Properties();
        properties.load(resourceAsStream);

        String name = properties.getProperty("name");
        String method = properties.getProperty("method");

        // 加载类，执行类的的方法
        Class aClass = Class.forName(name);
        Object object = aClass.newInstance();
        Method method1 = aClass.getMethod(method);

        method1.invoke(object);


        /**
         * 获取 class 对象的三个方式
         *
         * 注意：
         * 1. 2的方式，必须在类加载到内存的情况下
         * 2. 类只会被加载一次，也就是 Class 对象在内存里只有一个
         */
        // 1. Class.forName("全类名")
        // 2. 类名.class
        // 2. 对象.getClass()

    }
}
```

