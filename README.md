# My-SQL-book

1. 了解SQL 

    terms: 
        row - 横排，属性。也叫record。
        column - 数列，数据
        table - 上面东西构成的
        primary key - 独一性，不是NULL。特点：不更新，不重复，不改变（如果是供应商的名字，那么会经常需要改变）
    
2. MY SQL简介 

    一种DBMS- database management system。
    fast，reliable，simple
    运行方式：mysql
            MySQL Administration - 外部
            MySQL Query Browser - 外部
            
3. SHOW用法 

    SHOW COLUMNS FROM customers  =  DESCIBE customers
    功能一般大些，为好区分。
    相当于一层层往下看内容

4. SELECT - 检索 

    用“；”结束，分段是为了逻辑，不影响实际功能
    4.1）查单个
        SELECT prod_name FROM products;
        选中的会呈现出来，一般情况下可能是乱序出现的
        
    4.2）查多个 “，”分隔
        SELECT prod_name, prod_price FROM products;
        
    4.3）选所有项
        SELECT * FROM products;
        一般不怎么用，除非是所有列表都要用到
        
    4.4）找不重复的行
        SELECT DISTINCT vend_id FROM products；
        
    4.5）LIMIT 返回特定的值，一般限定行数
        SELECT prod_name FROM products        -- 你看这里没有分号
        LIMIT 5;          // 返回不多于5行
        LIMIT 5，5；      // 从5行开始，走5行。但是这里的5遵守index从0开始，所以是第6行
        LIMIT 3，4； = LIMIT 4 offset 3；     // 以防晕，offset有同样作用
          若是超出范围，则仅显示available的部分
        
    4.6）完全限定 - 后面再说
        SELECT products.prod_name FROM products 

5. ORDER BY 排序，用clause增加定义

    5.1）选单个
        SELECT prod_name FROM products
        ORDER BY prod_name；          // name：默认先按数字小大排序，然后字母顺序排序
        
    5.2）选多个
        SELECT prod_name，prod_price FROM products
        ORDER BY prod_name；          // 展现两列columns：name + price
    
    5.3）定向排序：升/降
        SELECT prod_name，prod_price FROM products
        ORDER BY prod_price DESC；   // DESC表示descendent下降；ASC- ascendent上升，默认
        
    5.4）多个排序要求：
        SELECT prod_name，prod_price FROM products
        ORDER BY prod_price DESC，prod_name；   // 先排价格，若相同再按名字
        
       ----- tips: dictionary的情况下：A & a是同一个意思 -----
        
    5.5）ORDER BY + LIMIT 选取最大、最小值
        SELECT prod_name，prod_price FROM products
        ORDER BY prod_price DESC，prod_name
        LIMIT 1；             // 仅限一行，所以就选出了特定的那个值 
    
6. WHERE 过滤数据 【过滤部分并不显示出来】  search criteria = filter condition
    
    常用数学的比较符号：<, >, =, <=, >=, <>, !=
    
    6.1）单个值 
        SELECT prod_name，prod_price FROM products
        WHERE prod_price = 2.50；    // 也是检测是否存在2.50；
                                     // 和ORDER BY连用的时候先WHERE
        
    6.2）找特殊的值
        SELECT prod_name，prod_price FROM products
        WHERE prod_name = “fuses”；  // string - “”
        WHERE prod_id ！= 1003 
        
    6.3）BETWEEN 范围
        SELECT prod_name，prod_price FROM products
        WHERE prod_price BETWEEN 5 AND 10；
        
    6.4）IS NULL 找空值
 
7. WHERE + （AND/OR/IN/NOT）高级搜索 + 组合

    7.1 [AND]
        SELECT prod_name，prod_price, vend_id
        FROM products
        WHERE vend_id = 1003 AND prod_price <= 10； 
                // 同时满足，并列出。 where里面的元素不一定要SELECT
        
        [OR]
        SELECT prod_name，prod_price, vend_id
        FROM products
        WHERE vend_id = 1003 OR prod_price <= 10； 
                // 满足一个即可
                
        [AND + OR] 顺序
        SELECT prod_name，prod_price, vend_id
        FROM products
        WHERE vend_id = 1003 OR vend_id = 1002 AND prod_price <= 10； 
        
        WHERE (vend_id = 1003 OR vend_id = 1002) AND prod_price <= 10； 
        
                // 由于AND 高于 OR， 如果没有括号
                // 那么第一行则为vend-id = 1002 and prod-price <= 10  
                
    7.2 IN ( range, range)
        SELECT prod_name，prod_price, vend_id
        FROM products
        WHERE vend_id IN (1002, 1003)
        ORDER BY prod_name；     // (1002, 1003, 1004)
    
    7.3 NOT (IN) = rule out
        SELECT prod_name，prod_price, vend_id
        FROM products
        WHERE vend_id NOT IN (1002, 1003)
        ORDER BY prod_name；     // 除了 ID=1002, 1003 的， 其他都出来
        
    
8. LIKE = Wild card - 筛选信息+ % or _，注意和 REGEXP区别
    8.1 LIKE + ' % '
        SELECT prod_name，prod_price
        FROM products
        WHERE prod_name LIKE 'jet%'；
                // 从products里面选两项，其中name里面含有jet。。
                // Output： Jetpack 1000
                //          Jetpack 2000
            
        SELECT prod_name，prod_price
        FROM products
        WHERE prod_name LIKE '%anvil%'；
                // 前后都有元素，也包括空格，见下部分
                // Output： 0.5 ton anvil
                //          1 ton anvil 
                //          2 ton anvil
        
        SELECT prod_name，prod_price
        FROM products
        WHERE prod_name LIKE 's%e'；
                // Output： same
                //          sale 
                //          swhatevere
    
    
    8.2 LIKE + ' _ ' （underscore）
        SELECT prod_name，prod_price
        FROM products
        WHERE prod_name LIKE '_ ton anvil'；
                // 但是_ 是单个占位符。只表示一个
                // Output： 
                //          1 ton anvil 
                //          2 ton anvil
        
        SELECT prod_name，prod_price
        FROM products
        WHERE prod_name LIKE '% ton anvil'；
                // Output： 0.5 ton anvil %配对个字符
                //          1 ton anvil 
                //          2 ton anvil
        不要过度使用通配符

9. REGEXP 正则表达 - 也是筛选 (regular expression)
   注意和 LIKE区别 以及 OR情况的使用

    9.1 REGEXP
        SELECT prod_name
        FROM products
        WHERE prod_name REGEXP '1000'
        ORDER BY prod_name；
                // Output： prod_name
                //          Jetpack 1000 
                
        SELECT prod_name
        FROM products
        WHERE prod_name REGEXP '000'
        ORDER BY prod_name；
                // Output： prod_name
                //          Jetpack 1000 
                //          Jetpack 2000
                
        ***
        TIPS： LIKE 和 REXGEXP的区别
        WHERE prod_name LIKE '1000'： 什么都不返回，需要配合%使用
        WHERE prod_name REGEXP '1000'：只要有1000就返回。
        So REGEXP = 1000% + %1000 + %1000%
                
    9.2 REGEXP + OR: '|', [123] ton,etc.
        SELECT prod_name
        FROM products
        WHERE prod_name REGEXP '1000|2000'
        ORDER BY prod_name；
                // Output： prod_name
                //          Jetpack 1000 
                //          Jetpack 2000 
        
        SELECT prod_name
        FROM products
        WHERE prod_name REGEXP '[123] ton'
        ORDER BY prod_name；
                // 注意：他们之间没有逗号，但还是 或 的意思
                // Output： prod_name
                //          1 ton anvil 
                //          2 ton anvil
                
        SELECT prod_name
        FROM products
        WHERE prod_name REGEXP '1|2|3 ton'
        ORDER BY prod_name；
                // 注意：这里表示：1 或 2 或 3 ton，所以不一样
                // Output： prod_name
                //          1 ton anvil 
                //          2 ton anvil
                //          Jetpack 1000 
                //          Jetpack 2000    
             
        
        SELECT prod_name
        FROM products
        WHERE prod_name REGEXP '[1-5] ton'
        ORDER BY prod_name；
                // 表示范围。你看：不要用数学思维哦，0.5 ton后面也符合哦！！！
                // Output： prod_name
                //          .5 ton anvil
                //          1 ton anvil 
                //          2 ton anvil
                
    9.3 匹配特殊字符：'.', '\\.', '\\\', '\\_' 
    
        SELECT vend_name
        FROM vendors
        WHERE vend_name REGEXP '.'
        ORDER BY vend_name；
                // . 表示匹配任何字符
                // Output： prod_name
                //          ACME 
                //          Furball Inc.
                
        SELECT vend_name
        FROM vendors
        WHERE vend_name REGEXP '\\.'
        ORDER BY vend_name；
                // Output： prod_name
                //          Furball Inc.
            
        OTHERS: \\f 换页
                \\n 换行
                \\r 回车
                \\t 制表
                \\v 纵向制表
    
10. 创建计算字段：CONCAT， AS
    concatenate - 拼接；AS - 新建一个column。
    
    10.1 Concat()其它DBMS用 + ｜｜来实现。
    
        SELECT Concat(vend_name, '(', vend_country, ')')
        FROM vendors
        ORDER BY vend_name;
            // 用逗号连接需要合起来的部分；特殊符号用冒号连起来
        
        SELECT Concat(RTrim(vend_name), ' (', RTrim(vend_country), ')')
        FROM vendors
        ORDER BY vend_name;
            // RTrim 裁剪右边所有空格；LTrim 裁左空格；Trim 裁两边空格
            
        SELECT Concat(RTrim(vend_name), ' (', RTrim(vend_country), ')') 
        AS vend_title
        FROM vendors
        ORDER BY vend_name;
            // 刚刚连在一起的内容放到新表格里：vend_title 里面
    
    10.2 Calculation of data
    
        SELECT prod_id, 
               quantity, 
               item_price, 
               quantity * item_price AS expanded_price
        FROM orderitems
        WHERE order_num = 20005;
            // 为了方便看，写成了竖向；最后一行方便运算；运算结果放在新格子里    


11. 使用函数（Soundex()；AddDate()）

    11.1 文本处理    left(), RTrim(), Lower(), Soundex()）
    
        SELECT cust_name, cust_contact
        FROM customers
        WHERE Soundex(cust_contact) = Soundex('Y Lie');
            // Soundex()匹配所有发音类似 Y.lie 的联系名,避免出错。比如拼写成了 Y Lee。
        
    11.2 日期和时间处理函数
    
        SELECT cust_id, order_num
        FROM orders
        WHERE Date(order_date) = '2005-09-01';
            // 良好的情况是用Date（）。虽然WHERE order_date = '2005-09-01'也可以，但是万一后面有其它的时刻：时分秒，则搜索不出来。而用Date则可以实现
            
        SELECT cust_id, order_num
        FROM orders
        WHERE Date(order_date) BETWEEN '2005-09-01' AND '2005-09-30';    
            // 也可以匹配时间范围
            
        SELECT cust_id, order_num
        FROM orders
        WHERE Year(order_date) = 2005 AND Month(order_date) = 9;    
            // 也可以匹配时间范围
     
    11.3 数值处理函数
    
        和python很像：Abs()， Tan(), Sqrt()

12. 汇总数据，这边处理过的数据都放在新的column里面
    aggregate function - 聚集函数
    
    12.1 AVG()
    
        SELECT AVG(prod_prices) AS avg_price
        FROM products;
        (WHERE vend_id = 1003;)
            // 将货品价格平均值放在avg_price表格里；
            // 加了WHERE特指ID=1003的平均值
    
    12.2 COUNT()
    
        SELECT COUNT(*) AS num_cust
        FROM customers;
            // COUNT(*) 所有数目计数，哪怕空值（NULL）占位
            // COUNT(cust_email) 对有值的进行计数  
    
    12.3 MAX（）， MIN（）， SUM（）-这个一般会带一个WHERE
    
    12.4 DISRTINCT： 去重
    
        SELECT AVG(DISTINCT prod_prices) AS avg_price
        FROM products
        WHERE vend_id = 1003
            // 放在括号里。那么重复数据只用一次
            
    12.5 多个函数连用
    
        SELECT COUNT(*) AS num_items,
               MIN(prod_price) AS price_min,
               MAX(prod_price) AS price_max,
               AVG(prod_price) AS price_avg
        FROM products;
          // 因为很多，所以竖向排列，也方便看
    
13. 分组数据
    COUNT（*）计数； GROUP BY分组，知道每一具体项的数字
    
    13.1 GROUP BY 分别计数
    
        SELECT vend_id, COUNT(*) AS num_prods 
        FROM products
        GROUP BY vend_id;
            // WHERE > GROUP BY > ORDER BY
        
    13.2 HAVING 过滤分组（与WHERE有区别）
    
        SELECT cust_id, COUNT(*) AS orders 
        FROM orders
        GROUP BY cust_id
        HAVING COUNT(*) >= 2;
            // WHERE一般过滤行，having过滤分组（计数？）
    
        SELECT vend_id, COUNT(*) AS num_prods 
        FROM products
        WHERE prod_price >= 10
        GROUP BY vend_id
        HAVING COUNT(*) >= 2;
            // 先选单价 >= 10；分别计数；计数里 >= 2
            
    13.3 GROUP BY 和 ORDER BY需要同时使用，确保排序
    
        SELECT order_num, 
               SUM(quantity * item_price) AS ordertotal
        FROM orderitems
        GROUP BY order_num
        HAVING SUM(quantity * item_price) >= 50
        ORDER BY order_num;
            // ⚠️ WHERE 和 HAVING 都❌使用别名（AS）
     
    13.4 SELECT 顺序
    
        SELECT
        FROM
        WHERE
            // 以上常规
        GROUP BY 
        HAVING
        ORDER BY 
            // order最后，因为处理好了之后排序才有意义呀。排序费时
        LIMIT

14. 子查询Sub query/SQL Server -- IN
    MySQL子查询是嵌套在另一个查询，类似定语从句。IN方便我们找范围
    
    14.1 定语从句合并
    
        SELECT order_num
        FROM orderitems
        WHERE prod_id = 'TNT2';

        SELECT cust_id
        FROM orders
        WHERE order_num IN (20005, 20007);

        SELECT cust_id
        FROM orders
        WHERE order_num IN (SELECT order_num
                            FROM orderitems
                            WHERE prod_id = 'TNT2');
           // 由内向外（）查询，所以先选 prod_id = 'TNT2'；
           // 在上面的范围里找符合条件的 cust_id

        SELECT cust_name, cust_contact 
        FROM customers
        WHERE cust_id IN (SELECT cust_id
                          FROM orders
                          WHERE order_num IN (SELECT order_num
                                              FROM orderitems
                                              WHERE prod_id = 'TNT2'));
           // 在刚刚的基础上找客户姓名和联系方式，套娃～
           // 子查询不限制嵌套子查询数目，但还是不要写太多
           // 下面见到的联结或更好一些
        
    14.2 计算字段使用 COUNT
        涉及correlated subquery:即外部查询的子查询
        
        SELECT COUNT(*) AS orders
        FROM orders
        WHERE cust_id = 10001
        
        SELECT cust_name,
               cust_state,
               (SELECT COUNT(*)
                FROM orders
                WHERE orders.cust_id = customers.cust_id) AS orders
                    // 完全限定列名：在不同列表中会有同样的名字
        FROM customers
        ORDER BY cust_name;
        
15. 联结表 - JOIN 
    把信息从多个/关联表格column里取出来。如：客户名为。。的货号。。等
    外键 foreign key是联结的关键； 可伸缩性 scale
    
    15.1 建立联结
    
        SELECT vend_name, prod_name, prod_price
        FROM vendors, products
        WHERE vendors.vend_id = products.vend_id
        ORDER BY vend_name, prod_name;
            // 在vend 和 product两个表里，共有vend.id，可连
            // 用完全限定名 WHERE很重要，见下
    
        Cartesian product 笛卡尔积 么得WHERE
        如果上述代码没有WHERE，则检索出来的行数是第一个行 X 第二个：不管配对。即：vend.name跑一遍，再prod.name跑一遍。毫无逻辑子
    
    15.2 内部联结 INNER JOIN 。。。 ON; equijoin
    
        SELECT vend_name, prod_name, prod_price
        FROM vendors INNER JOIN products
        ON vendors.vend_id = products.vend_id;
            // 和上面一样，不过突出了是内部的结构。
            // INNER JOIN 。。。 ON - 完全限定
         
     15.3 联结多个联结  
        
        SELECT prod_name, vend_name, prod_price, quantity
        FROM orderitems, products, vendors
        WHERE vendors.vend_id = products.vend_id
            AND orderitems.prod_id = products.prod_id
            AND order_num = 2005;
                // 显示编号20005的货品。
                // 看两两表格间的联结是什么     
                
        回顾 子查询（定语从句）：
        SELECT cust_name, cust_contact 
        FROM customers
        WHERE cust_id IN (SELECT cust_id
                          FROM orders
                          WHERE order_num IN (SELECT order_num
                                              FROM orderitems
                                              WHERE prod_id = 'TNT2'));
                                              
        SELECT cust_name, cust_contact（cust_id, order_num）
        FROM customers, orders, orderitems
        WHERE  customers.cust_id = orders.cust_id
            AND orders.order_num = orderitems.order_num
            AND prod_id = 'TNT2';
                // 非常棒！写对啦！
                // 但是注意，我们要的只是名+名；后面两个只是帮助定位

16. 高级联结
    
    16.1 使用表别名 AS - 常见缩写
    
        SELECT Concat(RTrim(vend_name),'(', RTrim(vend_country), ')') AS vend_title
        FROM vendors
        ORDER BY vend_name;
            // 缩短mysql语句
            // 允许在SELECT语句中多次使用相同表。主要见下面
            
        SELECT cust_name, cust_contact
        FROM customers AS c, orders AS o, orderitems AS oi
        WHERE c.cust_id = o.cust_id
            AND oi.order_num = o.order_num
            AND prod_id = 'TNT2';
    
       别名 - 内部引用/自联结时不会弄混
        
        SELECT prod_id, prod_name
        FROM products
        WHERE vend_id = (
                         SELECT vend_id
                         FROM products
                         WHERE prod_id = 'DTNR');
                         
        SELECT prod_id, prod_name
        FROM products AS p1, products AS p2
        WHERE p1.vend_id = p2.vend_id
            AND p2.prod_id = 'DTNR';
                    //   WHERE products.vend_id = products.vend_id用两次没有语法问题，但是mysql不知道我引用的是哪一个，所以分别定义。
                    //⚠️ 我们是从FROM开始定义 p1 和 p2
                    // 看好是哪里的prod.id = DTNR。
                                   
    16.2  
        SELECT c.*, o.order_num, o.order_date,
            oi.pro_id, oi.quantity, OI.item_price
        FROM customers AS c, orders AS o, orderitems AS oi
        WHERE c.cust_id = o.cust_id
            AND oi.order_num = o.order_num
            AND prod_id = 'FB';
    (P109 - 没完。什么*？)
    
    16.3 
    
    

17.

18. 
