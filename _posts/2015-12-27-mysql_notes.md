---
layout: post
title: "MySQL笔记"
description: 
modified: 2015-12-27
category: MySQL
tags: [MySQL]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

- 基本概念：
    - InnoDB是一个可靠地事务处理引擎，不支持全文本搜索
    - MyISAM是一个性能极高的引擎，支持全文本搜索，不支持事务处理
    - MEMORY在功能上等同于MyISAM，但由于数据存储在内存中，速度很快，特别适合临时表

    - 模式-schema

        - 关于数据库和表的布局及特性的信息

    - 命令必须“；”或“\g”结束
- Mysql命令行

    - 查看表信息
        - desc table
        - show columns from table;

    - 显示创建的SQL语句
        - show create database db_name;
        - show create table table_name;

    - 其它
        - show status  服务器状态信息
        - show grants  显示授权用户
        - show errors/show warnings 显示服务器错误或警告信息

    - 查询

        - 查询语句顺序
            - SELECT
            - FROM
            - WHERE
            - GROUP BY
            - HAVING
            - ORDER BY
            - LIMIT

        - 检索去重
            - \>SELECT DISTINCT col
            -  FROM tb_name

        - 限制结果数
            - \>SELECT col1
            -  FROM tb_name
            -  LIMIT 5, 5
            -  第一个为开始位置，初始为0.第二个为显示个数

            - 等价于LIMIT 5 OFFSET 5
        - order by

            - 指定排序方向
                - \>SELECT col1, col2
                -  FROM tb_name
                -  ORDER BY col1 DESC;【默认ASC】

                - 注意：如果想在多个列上排序，必须对每个列使用DESC

                - 注意：ORDER BY必须放在LIMIT之前
        - where

            - 指定查询范围, in操作符
                - \>SELECT col1
                -  FROM tb_name
                -  WHERE col1 IN (1001,1002)
        - like

            - 通配
                - \>SELECT col1
                -  FROM tb_name
                -  WHERE col1 LIKE ‘jet%’

                - %匹配0个或多个字符

        - 数据过滤regexp

            - 正则搜索
                - \>SELECT col1
                - FROM tb_name
                - WHERE col1 REGEXP ‘1000’
                - REGEXP ‘.000’
                - REGEXP对列值匹配

            - 进行or匹配
                - \>SELECT col1
                -  FROM tb_name
                -  WHERE col1 REGEXP ‘1000|2000’

            - 几个中的一个
                - select prod_id from products where prod_name regexp '[1|2]000';

            - 范围匹配
                - select prod_id from products where prod_name regexp '[1-5]000';

            - 匹配特殊字符，\ 进行转义

                - 必须使用\\为前导。 \\-
                - \>SELECT col1
                -  FROM tb_name
                -  WHERE col1 REGEXP ‘\\.’
            - like和 regexp
                - like整列匹配
                - regexp 列值内匹配
        - concat

            - 拼接字符
                - \>SELECT Concat(name, ‘ ----‘, age)
                -  FROM tb_name

            - 去除空白
                - \>SELECT Rtrim(name)
                -  FROM tb_name
                - Ltrim() Trim()

            - 使用别名
                - \>SELECT Concat(name, ‘---‘, age) AS info
                -  FROM tb_name

        - 文本函数
            - left()  串左边字符
            - length() 串长度
            - locate() 找出串的一个子串
            - lower() 转为小写
            - ltrim() 去掉左边空格
            - right() 返回串右边字符
            - rtrim() 去掉串右边空格
            - soundex() 返回字符串soundex值

                - 匹配发音类似的结果

            - upper() 大写

        - 日期函数
            - adddate() 增加一个日期-天或周
            - addtime() 增加一个时间
            - curdate() 返回当前日期
            - curtime() 返回当前时间
            - date() 返回日期时间的日期部分
            - datediff() 计算两个日期差
            - date_add() 高度灵活的日期运算函数
            - date_format() 返回一个格式化的日期或时间串
            - day() 返回一个日期的天数部分
            - dayofweek() 对于一个日期，返回对应的星期几
            - hour()
            - minute()
            - month()
            - now() 当前日期和时间
            - second()
            - time() 当前日期时间的时间部分
            - year()

        - 数值函数
            - abs()
            - cos()
            - exp() 指数
            - mod()
            - pi() 返回圆周率
            - rand() 随机数
            - sin()
            - sqrt()
            - tan()

        - 聚集函数
            - avg 平均
                - \>SELECT AVG(price) AS avg_price
                -  FROM tb_name
            - count 计数
                - select count(*) from products; #无论Null还是非空，均纳入计数
                - select count(prod_id) from products; #计数有值记录，忽略NULL值
            - max 最大
                - \>SELECT MAX(price) AS max_price
                -  FROM tb_name
            - min 最小
                - \>SELECT MIN(price) AS min_price
                -  FROM tb_name
            - sum 求和
                - \>SELECT SUM(quantity) AS total
                -  FROM tb_name
                - \#sum函数忽略值为NULL的行
        - group
            - >SELECT id, COUNT(*) AS num_prods
            -  FROM tb_name
            -  GROUP BY id

            - 注意：
                - 1.group by 可以包含任意数目的列
                - 2.group by 中每个列都必须是检索列或有效的表达式（但不能使聚集函数）
                - 3.除聚集函数外，select语句中的每个列都必须在group by子句中出现
                - 4.如果分组列有Null值，Null将作为一个分组返回
                - 5.group by 子句必须出现在where子句之后, order by 之前

            - 过滤分组
                - \>SELECT cust_id, COUNT(*) AS orders
                -  FROM orders
                -  GROUP BY cust_id
                -  HAVING COUNT(*) > 2
            - where和having区别
                - where在分组前过滤，having在分组后过滤

        - 子查询

            - 用于过滤
                - \>SELECT cust_id
                -  FROM orders
                -  WHERE order_num IN (SELECT order_num
                -                     FROM orderitems)

            - 作为字段
                - \>SELECT cust_name,
                -        cust_state,
                -        (SELECT COUNT(*)
                -         FROM orders
                -         WHERE orders.cust_id = customers.cust_id) AS orders
                -  FROM customers
                -  ORDER BY cust_name

        - 联结表

            - 内部联结
                - \>SELECT vend_name,prod_name,prod_price
                -  FROM vendors INNER JOIN products ON vendors.vend_id = products.vend_id

        - 高级联结表

            - 自联结
                - \>SELECT prod_id, prod_name
                -  FROM products
                -  WHERE vend_id = (SELECT vend_id FROM products
                -                 WHERE prod_id = ‘DTNTR’)

                - 等于
                - \>SELECT p1.prod_id, p1.prod_name
                -  FROM products AS p1, products AS p2
                -  WHERE p1.vend_id = p2.vend_id
                -         AND p2.prod_id = ‘DTNTR’

            - 外部联结
                - \>SELECT customers.cust_id, orders.order_num
                -  FROM customers LEFT OUTER JOIN orders
                -       ON customers.cust_id = orders.cust_id

        - 组合查询
            - UNION
                - \>SELECT vend_id, prod_id, prod_price
                -  FROM products
                -  WHERE prod_price <=5
                -  UNION
                -  SELECT vend_id, prod_id, prod_price
                -  FROM products
                -  WHERE vend_id IN (1001,1002)
            - UNION自动去除重复行
            - UNION ALL 保留

        - 全文本搜索(MYISAM支持)

            - 启用
                - \>CREATE TABLE productnotes(
                -  note_id int NOT NULL AUT_INCREMENT,
                -  note_text text NULL,
                -  FULLTEXT(note_text)

            - 进行全文本搜索
                - \>SELECT note_text
                -  FROM tb_name
                -  WHERE Match(note_text) Against(‘rabbit’)

            - 布尔文本搜索
                - \>SELECT note_text
                -  FROM productontes
                -  WHERE Match(note_text) Against(‘heavy’ IN BOOLEAN MODE)

    - 插入数据

        - 插入一般
            - \>INSERT INTO customers(cust_name,
            -                      cust_address)
            -  VALUES(‘Pep’, ‘100 main street’),
            -        (‘Tim’, ‘200 main Street’);

        - 高效率插入：

            - 使用单条insert语句插入多条比多个插入效率高
            - \>INSERT INTO customers(cust_name,
            -                      cust_address)
            -  VALUES(‘Pep’, ‘100 main street’),
            -        (‘Tim’, ‘200 main Street’)，
            -  (’Tom’, ‘200 main Street’);

        - 插入检出的数据
            - \>INSERT INTO customers(cust_name,
            -                      cust_address)
            -  SELECT cust_name, custaddress
            -  FROM custnew;

            - 根据列来对应，select使不使用列名无关

    - 更新
        - \>UPDATE customers
        -  SET cust_email = ‘a@fudd.com’
        -  WHERE cust_id = 10005

        - 发生错误继续进行的更新
            - >UPDATE IGNORE customers

    - 删除
        - \>DELETE FROM customers
        -  WHERE cust_id = 10006

    - 更快的删除

        - truncate table

            - 删除原表并新建

    - 表操作

        - 建表
            - \>CREATE TABLE customers(
            -    cust_id int NOT NULL AUTO_INCREMENT,
            -    cust_name char(50) NOT NULL,
            -    vend_city char(50) NULL,
            -    quantity int NOT NULL DEFAULT 1,
            -    PRIMARY KEY(cust_id)
            - )ENGINE=InnoDB

        - 更新表

            - 加字段
                - \>ALTER TABLE vendors
                -  ADD vend_phone CHAR(20)

            - 删除字段
                - \>ALTER TABLE tb1 DROP COLUMN names;

            - 改变列类型
                - \>ALTER TABLE infos CHANGE list list tinyint NOT NULL DEFAULT '0'

            - 加主键
                - \>ALTER TABLE tb1 ADD primary key(id)

            - 定义外键
                - ALTER TABLE order items  ADD CONSTRAINT fk_orderitems_orders FOREIGN KEY (order_num) REFERENCES orders (order_num);

            - 删除一个字段
                - \>ALTER TABLE tb1 DROP field_name

            - 增加自增长主键
                - alter table customers change id id not null auto_increment primary key;

            - 增加新字段并设置为主键
                - Alter TABLE tablename ADD new_field_id int(5) default 0 not null auto_increment ADD primary key(new_field_id)
                - ALTER TABLE example ADD ID INT NOT NULL;
                - ALTER TABLE example ADD UNIQUE(url)

                - \>ALTER TABLE vendors
                -  DROP COLUMN vend_phone
                - alter table syntax: http://dev.mysql.com/doc/refman/5.1/en/alter-table.html

        - 删除表

        - 清空表数据

        - 重命名表
            - \>RENAME TABLE customers2 TO customers;
            -  ALTER TABLE 'oldname' RENAME TO 'newname'
            - ALTER TABLE xxx to xxx,
            - xxx to xxx,
            - xxx to xxx;

- 视图（一般用于检索）

    - 为嘛使用

        - 重用SQL语句

        - 简化复杂的SQL操作

        - 使用表的组成部分

        - 保护数据

        - 更改数据格式和表示i

        - 本身不包含数据，返回的是从其他表检索出来的，与表同步

    - 规则限制

        - 唯一命名

        - 数目无限制

        - 创建时必须有权限

        - 可以嵌套
        - order by 会被select 中的覆盖

        - 不能索引、不能有关联的触发器

        - 可以和表一起使用

    - 使用
        - create view
            - create view pc as select cust_name, cust_contact, prod_id from customers cu, orders od, orderitems oi where cu.cust_id = od.cust_id and oi.order_num = od.order_num;
        - show create view view_name
        - drop view view_name

        - 更新

            - 先drop 后create
            - create orreplace view

            - 有以下操作的视图无法更新

                - 分组(使用group by和having)

                - 联结

                - 子查询

                - 并

                - 聚集函数
                - distinct

                - 导出列（计算）

- 存储过程：简单、安全、高性能
    - why use

        - 把处理封装在容易的使用单元，简化操作

        - 不要求反复建立一系列处理步骤，保证数据完整性，所有人使用同一存储过程

        - 简化对变动的管理

        - 提高性能

    - 创建存储过程
        - CREATE PROCEDURE productpricing()
        -  BEGIN
        - SELECT Avg(prod_price) AS priceavg
        - FROM products;
        -  END

        - 因为中间会用到“;”作为分隔符，因此使用创建语句会报错，因此使用之前先改变分隔符，方法
        - DELIMITER //
        - PS:DELIMITER必须大写！！！！！

    - 调用存储过程
        - call productpricing();

    - 删除存储过程
        - drop PROCEDURE productpricing if exists;

    - 使用参数
        - >CREATE PROCEDURE ordertotal(
        - IN onumber INT,
        - OUT ototal DECIMAL(8,2)
        - )
        - BEGIN
        - SELECT Sum(item_price*quality)
        - FROM orderitems
        - WHERE order_num = onumber
        - INTO ototal;
        - END;

    - 调用参数
        - \>CALL ordertotal(200005, @total;
        - \>SELECT @total;

    - 检查存储过程
        - show procedure status like '';

- 游标

    - 场景

        - 在检索出来的行中前进或后退多行；仅用于存储过程和函数

    - 定义

        - 是存储在MySQL服务器上的数据库查询，不是select语句，是被该语句检索出来的结果集

    - 创建
        - \>CREATE PROCEDURE processorders()
        -  BEGIN
        - DECLARE ordernumbers CURSOR
        - FOR
        - SELECT order_number FROM orders;
        - BEGIN ordernumbers;
        - FETCH ordernumbers INTO o;
        - CLOSE ordernumbers;
        -  END;

        - DECLARE CONTINUE HANDLER FOR SQLSTATE ‘02000’ SET done=1;
        - REPEAT
        -     FETCH ordernumbers INTO o;
        - END;

    - 打开游标
        - open xxx；

    - 关闭游标
        - close xxx；

- 触发器
    - MySQL响应以下任意语句而自动执行的一条MySQL语句
        - DELETE
        - INSERT
        - UPDATE

    - 创建触发器

        - 所需信息

            - 唯一的名字

            - 触发器关联的表

            - 应该响应的活动

            - 何时执行
        - \>CREATE TRIGGER newproduct AFTER INSERT ON products
        -  FOR EACH ROW SELECT ‘Product added’

    - 删除触发器
        - \>DROP TRIGGER newproduct;

    - 使用
        - INSERT触发器
            - \>CREATE TRIGGER neworder AFTER INSERT ON orders
            -  FOR EACH ROW SELECT NEW.order_num
        - DELETE触发器
            - \>CREATE TRIGGER deleteorder BEFORE DELETE ON orders
            -  FOR EACH ROW
            -  BEGIN
            - INSERT INTO archive_orders(order_num, order_date, cust_id)
            - VALUES(OLD.order_num, OLD.order_date, OLD.cust_id);
            -  END;
        - UPDATE触发器
            - \>CREATE TRIGGER updatevendor BEFORE UPDATE ON vendors
            -  FOR EACH ROW SET NEW.vend_state = Upper(NEW.vend_state);

- 事务

    - 基本概念
        - ACID
        - A,原子性，事务是一个原子操作单元，其对数据的修改，要么全执行，要么全不执行
        - C.一致性，事务开始和完成的时候，数据必须都保持一致状态（所有相关数据规则和内部数据结构）
        - I.隔离性，保证事务不受外部并发操作影响，即事务处理中间过程状态对外不可见
        - D.持久性，事务完成后，对数据修改时永久性的，及时出现系统故障也能够保持

    - 术语

        - 事务(transaction)：一组SQL语句

        - 回退(rollback)：撤销指定SQL语句
            - SQL语句
            - START TRANSACTION;
            - \#一批SQL语句
            - ROLLBACK； #回到开始事务之前

            - 哪些可以回退
                - insert、update、delete

            - 不可以回退：
                - select、create、drop

        - 提交：
            - START TRANSACTION;
            - \#SQL1
            - \#SQL2
            - COMMIT；

            - 若第一个成功，第二个失败，则不会提交
            - PS:COMMIT、ROLLBACK执行后事务自动关闭

        - 保留点：事务处理中设置的临时占位符
            - SAVEPOINT delete1;
            - ROLLBACK TO delete1；

- 全球化和本地化

    - 字符集和校对顺序

        - 字符集：字母和符号的集合

        - 编码：某个字符集成员的内部表示

        - 校对：规定字符如何比较指令

    - 使用字符集和校对顺序

        - 查看支持字符集列表
            - SHOW CHARACTER SET;

        - 查看支持的校对列表
            - SHOW COLLATION;

                - 区分大小写(_cs)

                - 不区分大小写(_ci)
        - SHOW VARIABLES LIKE 'character%';
        - SHOW VARIABLES LIKE 'collations%';

- 访问控制

    - 整个服务器:grant all和revoke all

    - 整个数据库:on database.*

    - 特定表：on database.table

    - 特定列

    - 特定存储过程

- 导入导出

    - 导入文本文件
        - \>LOAD DATA LOCAL INFILE 'd:/mysql.txt' INTO TABLE mytable;

    - 导入sql文件
        - \>use database;
        - \>source d:/mysql.sql

    - 备份
        - mysqldump [OPTIONS] database [tables]

        - 备份MySQL数据库的命令
            - mysqldump -hhostname -uusername -ppassword databasename > backupfile.sql

        - 备份MySQL数据库为带删除表的格式，能够让该备份覆盖已有数据库而不需要手动删除原有数据库。
            - mysqldump -–add-drop-table -uusername -ppassword databasename > backupfile.sql

        - 直接将MySQL数据库压缩备份
            - mysqldump -hhostname -uusername -ppassword databasename | gzip > backupfile.sql.gz

        - 备份MySQL数据库某个(些)表
            - mysqldump -hhostname -uusername -ppassword databasename specific_table1 specific_table2 > backupfile.sql

        - 同时备份多个MySQL数据库
            - mysqldump -hhostname -uusername -ppassword –databases databasename1 databasename2 databasename3 > multibackupfile.sql

        - 仅仅备份数据库结构
            - mysqldump –no-data –databases databasename1 databasename2 databasename3 > structurebackupfile.sql

        - 备份服务器上所有数据库
            - mysqldump –all-databases > allbackupfile.sql

    - 还原

        - 还原MySQL数据库的命令
            - mysql -hhostname -uusername -ppassword databasename < backupfile.sql
            - mysql -hhostname -ppassword databasename tablename < backuptablefile.sql

        - 还原压缩的MySQL数据库
            - gunzip < backupfile.sql.gz | mysql -uusername -ppassword databasename

        - 将数据库转移到新服务器
            - mysqldump -uusername -ppassword databasename | mysql –host=*.*.*.* -C databasename

        - 将查询结果导入外部文件
            - SELECT a,b,a+b
            - FROM test_table
            - INTO OUTFILE '/tmp/result.txt'
            - FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"'
            - LINES TERMINATED BY '\n'

            - 或者
            - mysql -u you -p -e "SELECT ..." >  file_name

- 数据库维护

    - 日志文件

        - 错误日志:hostname.err

        - 查询日志:hostname.log

        - 二进制日志：hostname-bin

        - 缓慢查询日志
            - hostname-slow.log

    - 实时监控

        - 查看mysql数据库的当前连接数

            - 命令： show processlist;

            - 或者 # mysqladmin -uroot -p密码 processlist

        - 当前状态

            - 命令： show status;

            - 或者 # mysqladmin -uroot -p密码 status

Q&A

- 数据库是否区分大小写以及排序顺序,A和a哪个在前哪个在后

    - 取决于数据库的设计，字典排序中，A与a相同，MySQL和大多数据库管理系统默认的
- SQL过滤与应用过滤

    - 应用过滤即在SQL选出超过实际需要的数据，然后客户机对返回数据进行循环，提取出需要的数据

    - 应该过滤缺点

        - 影响应用性能

        - 使创建的应用不具备可伸缩性

        - 使服务器通过网络发送多余数据，导致网络带宽的浪费

- 检测值是否区分大小写

    - 关键字不区分大小写，同时使用语句中也不区分大小写

- IN操作符优点

    - 使用长的合法选项清单时，更清楚直观

    - 计算次序更易管理
    - IN操作一般比OR操作快

    - 可以包含其他SELECT语句，能动态建立WHERE语句
- MySQL中的NOT

    - 支持使用NOT对IN、BETWEEN和EXISTS取反操作

- 解决尾空格对通配符搜索%的干扰

    - 在最后附加一个%

    - 使用函数去除尾空格

    - 不匹配NULL

- 下划线(_)通配符

    - 匹配单个字符

- 通配符使用技巧

    - 不要过度使用

        - 效率低下

    - 除非绝对必要，否则不要放在开始处
- LIKE与REGEXP（正则表达式）区别
    - LIKE匹配整个列，REGEXP在列值内进行匹配，若被匹配的文本在列值中出现将匹配
- REGEXP匹配区分大小写(REGEXP BINARY ‘xxx')
- MySQL对于年份的假定
    - 2位数字中处理00-69为2000~2069；处理70~99为1970~1999

- 如果对日期进行比较，使用Date()

- 匹配到月的方法(如201509)
    - Date(order_date) between ‘2015-09-01’ and '2015-09-30'
    - Year(x) = 2015 and Month(x) = 9;
- rollup

- having与where
    - where过滤行，having过滤分组
    - where在分组前过滤，having在分组后过滤

- 外键不能跨引擎