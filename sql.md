## pk = prime key 每个表建议都至少要有一个

## NN =  not none

```sql
INSERT INTO table_name

        VALUES (),(),()
```

**往多个表插入数据**

```sql
INSERT INTO orders (customers_id, order_date, status)

VALUES (1, '2019-01-02', 1);

INSERT INTO order_items

VALUES (LAST_INSERT_ID(), 1, 1, 2.95),

               (LAST_INSERT_ID(), 2, 1, 3.95)
```

**创建表复制**

```sql
CREATE TABLE new_table AS

SELECT * FROM old_table
```

**更新单行**

```sql
UPDATE table_name

SET column_name1 = xx, column_name2 = xx

WHERE prime_key = x
```

**更新多行**

```sql
WHERE condition
```

**在UPDATE中使用子查询**

```sql
WHERE client_id IN ()
```

**删除行**

```sql
DELETE FROM table_name
WHERE condition
```

**数值函数**

```sql
SELECT ROUND(5.73,1) -- 四舍五入
SELECT TRUNCATE(5.73,2) -- 截断
SELECT CEILING(5.7) -- 大于的最小整数
SELECT FLOOR(5.2) -- 小于的最大整数
SELECT ABS(-5.2) -- 绝对值
SELECT RAND() -- 0-1随机浮点数
```

**字符串函数**

```sql
SELECT LENGTH('sky')
SELECT UPPER('sky')
SELECT LOWER('Sky')
SELECT LTRIM('   sky')
SELECT RTRIM('sky   ')
SELECT TRIM('  sky  ')
SELECT LEFT('CHINA', 2) -- CH  RIGHT()
SELECT SUBSTRING('CHINA',2,3) -- HIN
SELECT LOCATE('n', 'CHINA') -- 不区分大小写
SELECT REPLACE('CHINA', 'CH','H')
SELECT CONCAT('first_name', ' ', 'last_name')
```

**日期函数**

```sql
SELECT NOW(), CURDATE(), CURTIME()
SELECT YEAR(NOW()) MONTH() DAY()  HOUR()  SECOND()
SELECT DATNAME(NOW()) -- 字符串格式星期，Monday
SELECT MONTHNAME(NOW())  -- 字符串格式月份，September
SELECT EXTRACT(YEAR FROM NOW())  --提取
```

**格式化日期和时间**

```sql
SELECT DATE_FORMAT(NOW(), '%M %d %Y') -- TIME_FORMAT()
```

**计算日期和时间**

```sql
SELECT DATE_ADD(NOW(), INTERVAL 1 YEAR)  -- DATE_SUB()
SELECT DATEDIFF('2023-09-07', '2023-09-01') -- 调换会得到负数
SELECT TIME_TO_SEC('20:14') - TIME_TO_SEC('20:10')
```

**IFNULL和COALESCE函数**

```sql
SELECT 
    order_id,
    IFNULL(shipper_id, 'NOT assigned') AS shipper
    COALESCE(shipper_id, comments, 'NOT assigned') AS shipper
-- IFNULL返回第二个，COALESCE返回第一个非空的
FROM orders
```

IF函数

```sql
SELECT 
    order_id,
    order_date,
    IF(YEAR(order_date) = YEAR(NOW()), 'Active', 'Archived')
FROM orders
```

CASE运算符

```sql
SELECT
    order_id,
    CASE
        WHEN YEAR(order_date) = YEAR(NOW()) THEN 'Active'
        WHEN YEAR(order_date) = YEAR(NOW()) - 1 THEN 'Last Year'
        WHEN YEAR(order_date) < YEAR(NOW()) - 1 THEN 'Archived'
        ELSE 'Future'
    END AS category
FROM orders
```

创建视图view

```sql
CREATE VIEW name AS
...
```

删除视图

```sql
DROP VIEW name
```

更改视图

```sql
CREATE OR REPLACE VIEW name AS
```

更新视图

```sql
-- DISTINCT
-- Aggregate Funtions
-- GROUP BY / HAVING
-- UNION
--没有以上语句称为可更新视图
```

**创建一个存储过程**

```sql
DELIMITER $$
CREATE PROCEDURE get_clients()
BEGIN
    SELECT * FROM clients;
END $$

DELIMITER ;
```

**调用一个存储过程**

```sql
CALL get_clients()
```

**删除存储过程**

```sql
DROP PROCEDURE IF EXISTS get_clients
```

**参数**

```sql
DROP PROCEDURE IF EXISTS get_clients_by_state;

DELIMITER $$
CREATE PROCEDURE get_clients_by_state
(
    state CHAR(2)
)
BEGIN
    SLELCT * FROM clients c
    WHERE c.state = state;
END $$

DELIMITER ;
```

**带默认值的参数**

```sql
DROP PROCEDURE IF EXISTS get_clients_by_state;

DELIMITER $$
CREATE PROCEDURE get_clients_by_state
(
    state CHAR(2)
)
BEGIN
    IF state IS NULL THEN
        SET state = 'CA';
    END IF;    

    SLELCT * FROM clients c
    WHERE c.state = state;  -- IFNULL(state,c.state)
END $$

DELIMITER ;
```

**参数验证**

```sql
CREATE PROCEDURE make_payment
(
    invoice_id INT,
    payment_amount DECIMAL(9,2),
    payment_date DATE
)
BEGIN
    IF payment_amout <= 0 THEN
        SIGNAL SQLSTATE '22002'
            SET MSEEAGE_TEXT = 'Invalid payment_amout';
    END IF
    UPDATE invoices i
    SET
        i.payment_total = payment_amount,
        i.payment_date = payment_date
    WHERE i.invoice_id = invoice_id;
END
```

**输出参数**

```sql
CREATE PROCEDURE get_unpaid_invoices_for_client
(
    client_id INT
    OUT invoices_count INT,
    OUT invoices_total DECIMAL(9,2)
)
BEGIN
    SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices i
    WHERE i.client_id = client_id
        AND payment_total = 0;
END
```

**变量**

```sql
CREATE PROCEDURE get_risk_factor()
BEGIN
    DECLARE risk_factor DECIMAL(9,2) DEFAULT 0;
    DECLARE invoices_total DECIMAL(9,2);
    DECLARE invoices_count INT;

    SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices;

    SET risk_factor = invoices_total/invoices_count * 5;

    SELECT risk_factor;
END
```

**函数**

```sql
CREATE FUNCTION get_risk_factor_for_client
(
    client_id INT
)
RETURNS INTEGER
--DETERMINISTIC
READS SQL DATA
--MODIFIES SQL DATA
BEGIN
    DECLARE risk_factor DECIMAL(9,2) DEFAULT 0;
    DECLARE invoices_total DECIMAL(9,2);
    DECLARE invoices_count INT;

    SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices
    WHERE i.client_id = client_id;

    SET risk_factor = invoices_total/invoices_count * 5;

    RETURN risk_factor;  -- RETURN IFNULL(risk_factor,0);
END
```

**删除函数**

```sql
DROP FUNCTION IF EXISTS get_risk_factor_for_client;
```

**触发器**

```sql
触发器是在插入、更新和删除语句前后自动执行的一堆SQL代码

DELIMITER $$
CREATE TRIGGER payments_after_insert
    AFTER INSERT ON payments
    FOR EACH ROW
BEGIN
    UPDATE invoices
    SET payment_total = payment_total + NEW.amount
    WHERE invoice_id = NEW.invoice_id;
END $$

DELIMITER ;
```

**显示触发器**

```sql
SHOW TRIGGERS
```

**删除触发器**

```sql
DROP TRIGGER IF EXISTS name
```

**使用触发器进行审计**

```sql
DELIMITER $$
CREATE TRIGGER payments_after_insert
    AFTER INSERT ON payments
    FOR EACH ROW
BEGIN
    UPDATE invoices
    SET payment_total = payment_total + NEW.amount
    WHERE invoice_id = NEW.invoice_id;

    INSERT INTO payments_audit
    VALUES (NEW.clinet_id, NEW.date, NEW.amount, 'INSERT', NOW());
END $$

DELIMITER ;
```

**事件**

```sql
DELIMITER $$

CREATE EVENT yearly_delete_stale_audit_rows
ON SCHEDULE
    EVERY 1 YEAR STARTS '2019-01-01' ENDS '2029-01-01'
DO BEGIN
    DELETE FROM payments_audit
    WHERE action_date < NOW() - INTERVAL 1 YEAR;
END $$

DELIMITER ;
```

**查看、删除和更改事件**

```sql
SHOW EVENTS;
DROP EVENT IF EXISTS  name;
ALTER EVENT name DISABLE;
```

**事务（Transactions）**

```sql
一个工作单元，内部语句都要成功运行
1.原子性(Atomicity)
2.一致性(Consistency)
3.隔离性(Isolation)
4.持久性(Durability)
mysql默认自动开启事务。但是mysql的自动事务是一条sql语句独占一个事务，
即用户执行一条sql完毕后会立即同步到数据表中，默认操作完后自动commit提交。
```

**创建事务**

```sql
START TRANSACTION;

INSERT INTO orders(customer_id, order_date,status)
VALUES(1,'2019-01-01', 1);

INSERT INTO order_items
VALUES (LAST_INSERT_ID(),1,1,1);

COMMIT;
-- ROLLBACK
```

**并发和锁定**

```sql
互斥锁
```

**隔离级别**

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED
SET TRANSACTION ISOLATION LEVEL READ COMMITTED
SET TRANSACTION ISOLATION LEVEL READ REPEATABLE
SET TRANSACTION ISOLATION LEVEL READ SERIALIZABLE
```

**死锁**

```sql
死锁的四个必要条件
```

**数据类型**

```sql
字符串类型
整数类型
定点小数类型
    DECIMAL(p,s) --DEC  NUMERIC  FIXED
浮点小数类型
    FLOAT DOUBLE
布尔类型
枚举和集合类型
日期和时间类型
    DATE
    TIME
    DATETIME 8b
    TIMESTAMP 4b(up to 2038)
    YEAR
BLOBS类型(存储二进制长对象，如图像，视频，pdf，word)
    TINYBLOB 255b
    BLOB 65KB
    MEDIUMBLOB 16MB
    LONGBLOB 4GB
JSON类型
```

**创建索引**

```sql
EXPLAIN ..
CREATE INDEX idx_point ON customers(points)
```

**查看索引**

```sql
主键会自动创建索引
SHOW INDEXES IN customers;
```

**全文索引**

```sql
CREATE FULLTEXT INDEX idx_title_body ON posts (title,body);
SELECT * , MATCH(title, body) AGAINST('react redux')
FROM posts
WHERE MATCH(title, body) AGAINST('react -redux +from' IN BOOLEAN MODE);
```

**复合索引**

```sql
CREATE INDEX idx_state_points ON customers (state,points);
DROP INDEX name ON table_name;
```

**复合索引中的列的顺序**

```sql
。
```
