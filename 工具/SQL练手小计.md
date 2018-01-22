## Mysql 必知必会

### 查询

1. 检索多个列

   ```
   SELECT
     prod_id,
     prod_name,
     prod_price
   FROM Products;
   ```

   注意，多个字段名称之间要用 `,` 分割，最后一个字段不需要 `,` 

2. 关于通配符 * 号

   一般而言，除非你确实需要表中的每一列，否则最好别使用 * 通配符。虽然使用通配符能让你自己省事，不用明确列出所需列，但检索不需 要的列通常会降低检索和应用程序的性能。

3. DISTINCT 关键字

   ```
   SELECT DISTINCT vend_id
   FROM Products;
   ```

   对取到的结果里面 `vend_id` 去重

   ```
   SELECT DISTINCT
     vend_id,
     prod_name
   FROM Products;
   ```

   去重，告诉DBMS只返回不同值，如果使用 DISTINCT 关键字，它必须直接放在列名的前面。
   DISTINCT 关键字作用于所有的列，不仅仅是跟在其后的那一列。例如，你指定 `SELECT DISTINCT vend_id, prod_price`，除非指定的两列完全相同，否则所有的行都会被检索出来。

4. LIMIT

   ```
   SELECT prod_name
   FROM Products
   LIMIT 5;
   ```

   只取5行数据

   ```
   SELECT prod_name
   FROM Products
   LIMIT 5 OFFSET 1;
   ```

   取5行数据，从第一行开始(注意：数据库是从0开始计数行的)

   ```
   SELECT prod_name
   FROM Products
   LIMIT 5, 1;
   ```

   前一个数字代表从第几行开始，后一个数字代表取几个数据

5. 注释

   一共有三种类型

   ```
   /**
     注释
    */
    
   -- 注释

   # 注释
   ```

6. ORDER BY

   + 按单个列排序
   + 按多个列排序
   + 按照列位置排序

   ORDER BY子句的位置
   在指定一条ORDER BY子句时，应该保证它是SELECT语句中最后一条子句。如果它不是最后的子句，将会出现错误消息。

   通过非选择列进行排序
   通常，ORDER BY子句中使用的列 SELECT 选择的列。但是，实际上并不一定要这样，用其他列也是完全合法的

   ```
   SELECT
     prod_id,
     prod_price,
     prod_name
   FROM Products
   ORDER BY prod_price, prod_name;
   ```

   先按照 prod_price 进行排序，当价格相同的情况下，使用 prod_name 进行排序

   ```
   SELECT prod_id, prod_price, prod_name FROM Products
   ORDER BY 2, 3;
   ```

   ORDER BY 2表示按SELECT清单中的第二个列prod_name进行排序。ORDER BY 2，3表示先按prod_price，再按prod_name进行排序。(不建议使用这种方式，可读性比较差)

7. DESC 和 ASC

   ```
   SELECT
     prod_id,
     prod_price,
     prod_name
   FROM Products
   ORDER BY prod_price DESC, prod_name;
   ```

   DESC 降序，跟在字段名称后面

   DESC是 DESCENDING 的缩写，这两个关键字都可以使用。与DESC相对的是ASC(或ASCENDING)，在升序排序时可以指定它。但实际 上，ASC没有多大用处，因为升序是默认的。

   Q:在对文本性数据进行排序时，A与a相同吗?a位于B之前，还是Z之后 ? 

   A:这些问题不是理论问题，其答案取决于数据库的设置方式。

   在字典(dictionary)排序顺序中，A被视为与a相同，这是大多数数据库管理系统的默认行为。但是，许多DBMS允许数据库管理员在需要时改变这种行为(如果你的数据库包含大量外语字符，可能必须这样做)。

8. WHERE 字句
   数据库表一般包含大量的数据，很少需要检索表中的所有行。通常只会根据特定操作或报告的需要提取表数据的子集。只检索所需数据需要指定搜索条件(search criteria)，搜索条件也称为过滤条件(filter condition)。

   **提示**:

   SQL过滤与应用过滤 数据也可以在应用层过滤。为此，SQL的SELECT语句为客户端应用检索出超过实际所需的数据，然后客户端代码对返回数据进行循环，提取出需要的行。
   通常，这种做法极其不妥。优化数据库后可以更快速有效地对数据进行过滤。而让客户端应用(或开发语言)处理数据库的工作将会极大地 影响应用的性能，并且使所创建的应用完全不具备可伸缩性。此外，如果在客户端过滤数据，服务器不得不通过网络发送多余的数据，这将导致网络带宽的浪费。

   ```
   SELECT
     prod_name,
     vend_id,
     prod_price
   FROM Products
   WHERE vend_id != 'DLL01'
   ```

   提示: 何时使用引号 ? 如果仔细观察上述 WHERE 子句中的条件，会看到有的值括在单引号内，而有的值未括起来。单引号用来限定字符串。如果将值与字符串类型的列进行比较，就需要限定引号。用来与数值列进行比较的值不用引号。

   ```
   SELECT
     prod_name,
     prod_price
   FROM Products
   WHERE prod_price BETWEEN 5 AND 10;
   ```

   从这个例子可以看到，在使用BETWEEN时，必须指定两个值——所需范围的低端值和高端值。这两个值必须用AND关键字分隔。BETWEEN匹配范围 中所有的值，包括指定的开始值和结束值。

9. NULL

   在创建表时，表设计人员可以指定其中的列能否不包含值。在一个列不包含值时，称其包含空值NULL。

   **NULL**
   **无值(no value)，它与字段包含0、空字符串或仅仅包含空格不同**。

   ```
   SELECT cust_name
   FROM Customers
   WHERE cust_email IS NULL;
   ```

   确定值是否为NULL，不能简单地检查是否= NULL。SELECT语句有一个特殊的WHERE子句，可用来检查具有NULL值的列。这个WHERE子句就是IS NULL子句。

10. AND

    ```
    SELECT
      prod_id,
      prod_price,
      prod_name
    FROM Products
    WHERE vend_id = 'DLL01' AND prod_price < 5;
    ```

    此SQL语句检索由供应商DLL01制造且价格小于等于4美元的所有产品的名称和价格。这条SELECT语句中的WHERE子句包含两个条件，用AND关键 字联结在一起。AND指示DBMS只返回满足所有给定条件的行。如果某个产品由供应商DLL01制造，但价格高于4美元，则不检索它。类似地，如 果产品价格小于4美元，但不是由指定供应商制造的也不被检索

11. OR

    符合任何一个条件的结果都会被检索出来

    ```
    SELECT
      prod_name,
      vend_id,
      prod_price
    FROM Products
    WHERE vend_id = 'DLL01' OR vend_id = 'BRS01'
    ```

    把厂商是 DLL01 或者是 BRS01 的商品检索出来

12. 求值顺序

    ```
    SELECT
      prod_name,
      vend_id,
      prod_price
    FROM Products
    WHERE (vend_id = 'DLL01' OR vend_id = 'BRS01') AND prod_price > 10;
    ```

    当 AND 和 OR 搭配使用的时候，需要注意他们的优先级，AND 的优先级要比 OR 的高，所以有时候需要用 () 来约束查询子条件
    假如需要列出价格为10美元及以上，且由 DLL01 或 BRS01 制造的所有产品，需要用到下面的 SQL

13. IN

    ```
    SELECT
      prod_name,
      vend_id,
      prod_price
    FROM Products
    WHERE vend_id IN ('DLL01', 'BRS01')
    ```

    ```
    SELECT
      prod_name,
      vend_id,
      prod_price
    FROM Products
    WHERE vend_id = 'DLL01' OR vend_id = 'BRS01'
    ```

    上面两个 SQL 是一个意思

    为什么要使用IN操作符?其优点为:

    1. 在有很多合法选项时，IN操作符的语法更清楚，更直观。 
    2. 在与其他AND和OR操作符组合使用IN时，求值顺序更容易管理。 
    3. IN操作符一般比一组OR操作符执行得更快(在上面这个合法选项很少的例子中，你看不出性能差异)。 
    4. IN的最大优点是可以包含其他SELECT语句，能够更动态地建立WHERE子句。第11课会对此进行详细介绍。

14. NOT

    ```
    SELECT
      prod_name,
      vend_id,
      prod_price
    FROM Products
    WHERE NOT vend_id = 'DLL01'
    ```

    WHERE子句中的NOT操作符有且只有一个功能，那就是否定其后所跟的任何条件。因为NOT从不单独使用(它总是与其他操作符一起使用)，所以
    它的语法与其他操作符有所不同。NOT关键字可以用在要过滤的列前，而不仅是在其后。

    **为什么使用NOT?**
    对于这里的这种简单的WHERE子句，使用NOT确实没有什么优势。但在更复杂的子句中，NOT是非常有用的。例如，在与IN操作 符联合使用时，NOT可以非常简单地找出与条件列表不匹配的行。

15. 通配符

    1. %

       ```
       SELECT
               prod_name,
               prod_price
           FROM Products
           WHERE prod_name LIKE 'Fish%';
       ```

       在搜索串中，%表示任何字符出现任意次数

    2. _

       ```
        SELECT
               prod_name,
               prod_price
           FROM Products
           WHERE prod_name LIKE '__ inch teddy bear';
       ```

       下划线的用途与%一样，但它只匹配单个字符，而不是多个字符

    **注意：**

    不要过度使用通配符。如果其他操作符能达到相同的目的，应该使用其他操作符。 

    在确实需要使用通配符时，也尽量不要把它们用在搜索模式的开始处。把通配符置于开始处，搜索起来是最慢的。 仔细注意通配符的位置。如果放错地方，可能不会返回想要的数据。

16. 算数计算

    ```
    SELECT
      prod_id,
      quantity,
      item_price,
      quantity * item_price AS expanded_price
    FROM OrderItems
    WHERE order_num = 20008
    ```

    可以在查询的字段里面执行 加减乘除 运算

17. 文本函数

    1. upper 将字符串转换为大写

       ```
       SELECT
               vend_name,
               upper(vend_name) AS vend_name_upper
           FROM Vendors;
       ```

    2. LTRIM() 去掉字符串左边的空格

    3.  RTRIM() 去掉字符串右边的空格

    4. CONCAT() 连接字符串

       ```
       SELECT concat(vend_name, '(', vend_country, ')')
           FROM Vendors;
       ```

       可以把搜索的数据进行一个格式化，省了一步在java中的操作

18. 时间函数

    例如数据值为：2012-05-01 00:00:00 ，下面每个函数取时间里面不同的年月日

    ```
    year()
    month()
    day()
    ```

    ```
    SELECT
    	order_num,
    	cust_id
    	FROM Orders
    WHERE year(order_date) = 2012
    ```

19. 数值处理函数

    ABS() 返回一个数的绝对值

    SQRT() 返回一个数的平方根

20. 统计函数

    - AVG() 返回某列的平均值

    ```
    SELECT avg(prod_price) AS avg_price
        FROM Products
        WHERE vend_id = 'DLL01'
    ```

    **注意：**

    1. AVG() 只能用来确定特定数值列的平均值，而且列名必须作为函数参数给出。为了获得多个列的平均值，必须使用多个AVG()函数。

    2. AVG() 函数忽略列值为NULL的行

       ​

    - COUNT() 返回某列的行数

    1. 使用COUNT(*)对表中行的数目进行计数，不管表列中包含的是空值(NULL)还是非空值。
    2. 使用COUNT(column)对特定列中具有值的行进行计数，忽略NULL值。

    ```
    SELECT count(*)
    FROM Customers;

    SELECT count(cust_email)
    FROM Customers;
    ```

    ​

    - max() / min()

    MAX()一般用来找出最大的数值或日期值,MIN() 相反

    ```
    SELECT max(prod_price) AS max_price
    FROM Products;
    ```

    - sum()

    ```
    SELECT sum(quantity * OrderItems.item_price) AS total_price
    FROM OrderItems
    WHERE order_num = '20008';
    ```

    - DISTINCT 在函数中的作用

    ```
    SELECT avg(DISTINCT prod_price) AS avg_distinct
    FROM Products;
    ```

    这里先对 prod_price 进行一遍去重，然后再计算平均值

    - 组合函数使用

    ```
    SELECT
    	count(*)        AS num_items,
    	max(prod_price) AS price_min,
    	AVG(prod_price) AS price_avg
    FROM Products;
    ```

21. GROUP BY

    1. 创建分组

       ```
       SELECT
         vend_id,
         count(*) AS num_prod
       FROM Products
       GROUP BY vend_id;
       ```

    2. 过滤分组

       ```
       SELECT
         vend_id,
         count(*) AS num_prod
       FROM Products
       WHERE prod_price > 4
       GROUP BY vend_id
       HAVING count(*) > 2;
       ```

       HAVING 对分组后的数据进行过滤

    **注意：**

    1. GROUP BY子句可以包含任意数目的列，因而可以对分组进行嵌套，更细致地进行数据分组。
    2. 如果在GROUP BY子句中嵌套了分组，数据将在最后指定的分组上进行汇总。换句话说，在建立分组时，指定的所有列都一起计算(所以不能从个别的列取回数据)。
    3. GROUP BY子句中列出的每一列都必须是检索列或有效的表达式(但不能是聚集函数)。如果在SELECT中使用表达式，则必须在GROUP BY子 句中指定相同的表达式。不能使用别名。
    4. 大多数SQL实现不允许GROUP BY列带有长度可变的数据类型(如文本或备注型字段)。 除聚集计算语句外，SELECT语句中的每一列都必须在GROUP BY子句中给出。 如果分组列中包含具有NULL值的行，则NULL将作为一个分组返回。如果列中有多行NULL值，它们将分为一组。
    5. GROUP BY子句必须出现在WHERE子句之后，ORDER BY子句之前。

22. 子查询

    利用子查询进行过滤：
    Q：列出订购物品RGAN01的所有顾客

    ```
    SELECT
      cust_name,
      cust_contact
    FROM Customers
    WHERE cust_id IN (SELECT cust_id
                      FROM Orders
                      WHERE order_num IN (SELECT order_num
                                          FROM OrderItems
                                          WHERE prod_id = 'RGAN01'));
    ```

    **警告:**
    只能单列作为子查询的SELECT语句,企图检索多个列将返回错误
    **性能：**
    在WHERE子句中使用子查询能够编写出功能很强且很灵活的SQL语句。对于能嵌套的子查询的数目没有限制，不过在实际使用时由于性能 的限制，不能嵌套太多的子查询。

23. 连接

    ```
    SELECT
      vend_name,
      prod_name,
      prod_price
    FROM Vendors, Products
    WHERE Vendors.vend_id = Products.vend_id;
    ```

    返回笛卡儿积的联结，也称叉联结(cross join)

    使用 WHERE 条件就是对两个表形成的笛卡尔积进行一次筛选

    **笛卡儿积(cartesian product)** 
    由没有联结条件的表关系返回的结果为笛卡儿积。检索出的行的数目将是第一个表中的行数乘以第二个表中的行数。

    ​

    内连接

    ```
    SELECT
      vend_name,
      prod_name,
      prod_price
    FROM Vendors
      INNER JOIN Products ON Vendors.vend_id = Products.vend_id;
    ```

    下面两种SQL执行出来的结果是一样的

    ```
    SELECT
      cust_name,
      cust_contact
    FROM Customers
    WHERE cust_id IN (SELECT cust_id
                      FROM Orders
                      WHERE order_num IN (SELECT order_num
                                          FROM OrderItems
                                          WHERE prod_id = 'RGAN01'));
    ```

    ```
    SELECT
      cust_name,
      cust_contact
    FROM Customers, Orders, OrderItems
    WHERE Customers.cust_id = Orders.cust_id
          AND Orders.order_num = OrderItems.order_num
          AND prod_id = 'RGAN01';
    ```

    ​

    自连接

    Q:给与Jim Jones同一公司的所有顾客发送一封信件

    ```
    SELECT
      c1.cust_name,
      c1.cust_contact,
      c1.cust_email
    FROM Customers AS c1, Customers AS c2
    WHERE c1.cust_name = c2.cust_name
          AND c2.cust_contact = 'Jim Jones';
    ```

    自联结通常作为外部语句，用来替代从相同表中检索数据的使用子查询语句。虽然最终的结果是相同的，但许多DBMS处理联结远比处理子 查询快得多。应该试一下两种方法，以确定哪一种的性能更好。

    ​

    外链接

    ```
    SELECT
      Customers.cust_id,
      Orders.order_num
    FROM Customers
      LEFT JOIN Orders ON Customers.cust_id = Orders.cust_id
    ```

    上面的例子使用LEFT OUTER JOIN从FROM子句左边的表(Customers表) 中选择所有行。为了从右边的表中选择所有行，需要使用RIGHT OUTER JOIN

    Q:统计每个客户的订单数

    ```
    SELECT
      Customers.cust_id,
      count(order_num) AS order_num
    FROM Customers
      LEFT JOIN Orders ON Customers.cust_id = Orders.cust_id
    GROUP BY Orders.cust_id;
    ```

24. 插入数据

    ```
    INSERT INTO Customers (cust_id,
                           cust_name,
                           cust_address,
                           cust_city,
                           cust_state,
                           cust_zip,
                           cust_country)
    VALUES ('1000000006',
            'Toy Land',
            '123 Any Street',
            'New York',
            'NY',
            '11111',
            'USA');
    ```

    注意：要写列名，不要一套数据库的表字段结构

    **INSERT SELECT**
    可以把查询到的数据，一次性的插入，了解即可

    **提示:**
    INSERT SELECT 中的列名 为简单起见，这个例子在INSERT和SELECT语句中使用了相同的列名。但是，不一定要求列名匹配。事实上，DBMS一点儿也不关心SELECT返 回的列名。它使用的是列的位置，因此SELECT中的第一列(不管其列名)将用来填充表列中指定的第一列，第二列将用来填充表列中指定的 第二列，如此等等。

25. 复制表

    ```
    CREATE TABLE CustNew AS
      SELECT *
      FROM Customers;
    ```

    创建一个表 CustNew ，把 Customers 内的数据复制一份过去

    任何SELECT选项和子句都可以使用，包括WHERE和GROUP BY; 可利用联结从多个表插入数据; 不管从多少个表中检索数据，数据都只能插入到一个表中。

    提示:进行表的复制
    复制表数据是试验新SQL语句前进行表复制的很好工具。先进行复制，可在复制的数据上测试SQL代码，而不会影响实际的数据。

26. 更新表

    Q: 修改id是 1000000001 的顾客邮箱

    ```
    UPDATE Customers
    SET cust_email = 'zz@qq.com'
    WHERE cust_id = '1000000001';
    ```

    ```
    UPDATE Customers
    SET cust_email = 'zz@qq.com',
      cust_name    = 'zl'
    WHERE cust_id = '1000000001';
    ```

27. 删除表数据

    ```
    DELETE FROM Customers
    WHERE cust_id = '1000000006';
    ```

    **重要：**
    除非确实打算更新和删除每一行，否则绝对不要使用不带WHERE子句的UPDATE或DELETE语句。 
    保证每个表都有主键(如果忘记这个内容，请参阅第12课)，尽可能像WHERE子句那样使用它(可以指定各主键、多个值或值的范围)。 
    在UPDATE或DELETE语句使用WHERE子句前，应该先用SELECT进行测试，保证它过滤的是正确的记录，以防编写的WHERE子句不正确。 
    使用强制实施引用完整性的数据库(关于这个内容，请参阅第12课)，这样DBMS将不允许删除其数据与其他表相关联的行。 
    有的DBMS允许数据库管理员施加约束，防止执行不带WHERE子句的UPDATE或DELETE语句。如果所采用的DBMS支持这个特性，应该使用它。

28. 视图

    ```
    CREATE VIEW CustomProducts AS
      SELECT
        cust_name,
        cust_contact,
        prod_id
      FROM Customers, Orders, OrderItems
      WHERE Customers.cust_id = Orders.cust_id
            AND OrderItems.order_num = Orders.order_num;
    ```

    视图为虚拟的表。它们包含的不是数据而是根据需要检索数据的查询。视图提供了一种封装SELECT语句的层次，可用来简化数据处理，重新格式化或保护基础数据。


    ​				
    ​			
    ​		
    ​	