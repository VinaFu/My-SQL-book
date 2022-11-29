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
    
    5.2）选多个
    
    5.3）多个排序要求

6. WHERE 

7. WHERE + AND/OR/IN/NOT
