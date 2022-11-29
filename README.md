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
 
7. WHERE + （AND/OR/IN/NOT）

7. WHERE + AND/OR/IN/NOT
