【第二章】在单一表格中检索数据
===========
### 1. 选择语句
<iframe src="//player.bilibili.com/player.html?aid=47123168&bvid=BV1Xb41177na&cid=83008107&page=5" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe> 

##### 导航

第1节先看一下选择语句整体是什么样子，本章后面的小节会分别讲解其中各子句的具体写法。
##### 实例
```sql
<p>
USE sql_store;

SELECT * / 1, 2  -- 纵向筛选列，甚至可以是常数
FROM customers  -- 选择表
WHERE customer_id < 4  -- 横向筛选行
ORDER BY first_name  -- 排序

-- 单行注释

/*
多行注释
*/
</p>
```
### 2. 选择子句
<iframe src="//player.bilibili.com/player.html?aid=47123168&bvid=BV1Xb41177na&cid=83008125&page=6" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe> 

##### 小结
SELECT 是列/字段选择语句，可选择列，列间数学表达式，特定值或文本，可用AS关键字设置列别名（AS可省略），注意 DISTINCT 关键字的使用。

##### 注意
SQL会完全无视大小写（绝大数情况下的大小写）、多余的空格（超过一个的空格）、缩进和换行，SQL语句间完全由分号 ; 分割，用缩进、换行等只是为了代码看着更美观结构更清晰，这些与Python很不同，要注意。

##### 实例
```sql
<p>
USE sql_store;

SELECT
    DISTINCT last_name, 
    -- 这种选择的字段中部分取 DISTINCT 的是如何筛选的？
    first_name,
    points,
    (points + 70) % 100 AS discount_factor/'discount factor'
    -- % 取余（取模）
FROM customers
</p>
```

### 练习
单价涨价10%作为新单价

```sql
<p>
SELECT 
    name, 
    unit_price, 
    unit_price * 1.1 'new price'  
FROM products
</p>
```
如上面这个例子所示，取别名时，AS 可省，空格后跟别名就行，可看作是SQL会将将列变量及其数学运算之后的第一个空格识别为AS

### 3. WHERE子句
The WHERE Clause (5:17)

##### 小结
WHERE 是行筛选条件，实际是一行一行/一条条记录依次验证是否符合条件，进行筛选

##### 导航
3~9 节讲的都是写 WHERE 子句中表达筛选条件的不同方法，这一节（第3节）主要讲比较运算，第4节讲逻辑运算 AND、OR、NOT，5~9可看作都是在讲特殊的比较运算（是否符合某种条件）：IN、BETWEEN、LIKE、REGEXP、IS NULL。

所以总的来说WHERE条件就是数学→比较→逻辑运算，逻辑层次和执行优先级也是按照这三个的顺序来的。

##### 实例
```sql
<p>
USE sql_store;

SELECT *
FROM customers
WHERE points > 3000  
/WHERE state != 'va'  -- 'VA'/'va'一样
</p>
```
比较运算符 > < = >= <= !=/<> ，注意等于是一个等号而不是两个等号

也可对日期或文本进行比较运算，注意SQL里日期的标准写法及其需要用引号包裹这一点

WHERE birth_date > '1990-01-01'

##### 练习
今年（2019） 的订单 ？
```sql
<p>
USE sql_store;

select *
from orders
where order_date > '2019-01-01'
-- 有更一般的方法，不用每年改代码，之后教
</p>
```