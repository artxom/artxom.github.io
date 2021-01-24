
【第二章】在单张表中检索数据
===========
## 1. 查询语句
<iframe height="450" width="800" src="//player.bilibili.com/player.html?aid=47123168&bvid=BV1Xb41177na&cid=83008107&page=5" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe> 

### 导航

第1节先看一下查询语句整体是什么样子，本章后面的小节会分别讲解其中各子句的具体写法。
### 实例
```sql

USE sql_store;

SELECT * / 1, 2  -- 纵向筛选列，甚至可以是常数
FROM customers  -- 查询表
WHERE customer_id < 4  -- 横向筛选行
ORDER BY first_name  -- 排序

-- 单行注释

/*
多行注释
*/

```
## 2. 查询子句
<iframe height="450" width="800"  src="//player.bilibili.com/player.html?aid=47123168&bvid=BV1Xb41177na&cid=83008125&page=6" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe> 

### 小结
SELECT 是列/字段查询语句，可查询列，列间数学表达式，特定值或文本，可用AS关键字设置列别名（AS可省略），注意 DISTINCT 关键字的使用。

### 注意
SQL会完全无视大小写（绝大数情况下的大小写）、多余的空格（超过一个的空格）、缩进和换行，SQL语句间完全由分号 ; 分割，用缩进、换行等只是为了代码看着更美观结构更清晰，这些与Python很不同，要注意。

### 实例
```sql

USE sql_store;

SELECT
    DISTINCT last_name, 
    -- 这种查询的字段中部分取 DISTINCT 的是如何筛选的？
    first_name,
    points,
    (points + 70) % 100 AS discount_factor/'discount factor'
    -- % 取余（取模）
FROM customers

```

## 练习
单价涨价10%作为新单价

```sql

SELECT 
    name, 
    unit_price, 
    unit_price * 1.1 'new price'  
FROM products

```
如上面这个例子所示，取别名时，AS 可省，空格后跟别名就行，可看作是SQL会将将列变量及其数学运算之后的第一个空格识别为AS

## 3. WHERE子句
<iframe height="450" width="800"  src="//player.bilibili.com/player.html?aid=47123168&bvid=BV1Xb41177na&cid=83131403&page=7" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe> 

### 小结
WHERE 是行筛选条件，实际是一行一行/一条条记录依次验证是否符合条件，进行筛选

### 导航
3到9 节讲的都是写 WHERE 子句中表达筛选条件的不同方法，这一节（第3节）主要讲比较运算，第4节讲逻辑运算 AND、OR、NOT，5到9可看作都是在讲特殊的比较运算（是否符合某种条件）：IN、BETWEEN、LIKE、REGEXP、IS NULL。

所以总的来说WHERE条件就是数学→比较→逻辑运算，逻辑层次和执行优先级也是按照这三个的顺序来的。

### 实例
```sql

USE sql_store;

SELECT *
FROM customers
WHERE points > 3000  
/WHERE state != 'va'  -- 'VA'/'va'一样

```
比较运算符 > < = >= <= !=/<> ，注意等于是一个等号而不是两个等号

也可对日期或文本进行比较运算，注意SQL里日期的标准写法及其需要用引号包裹这一点

WHERE birth_date > '1990-01-01'

### 练习
今年（2019） 的订单 ？
```sql

USE sql_store;

select *
from orders
where order_date > '2019-01-01'
-- 有更一般的方法，不用每年改代码，之后教

```
## 4. AND, OR, NOT运算符
<iframe height="450" width="800"  src="//player.bilibili.com/player.html?aid=47123168&bvid=BV1Xb41177na&cid=83131430&page=8" scrolling="no" border="0" frameborder="no"  framespacing="0" allowfullscreen="true" > </iframe> 

### 小结
用逻辑运算符AND、OR、NOT对（数学和）比较运算进行组合实现多重条件筛选
执行优先级：数学→比较→逻辑

### 实例
```sql
USE sql_store;

SELECT *
FROM customers
WHERE birth_date > '1990-01-01' AND points > 1000
/WHERE birth_date > '1990-01-01' OR 
      points > 1000 AND state = 'VA'
```
AND优先级高于OR，但最好加括号，更清晰
```sql
WHERE birth_date > '1990-01-01' OR 
      (points > 1000 AND state = 'VA')
```
NOT的用法
```sql
WHERE NOT (birth_date > '1990-01-01' OR points > 1000)
```
去括号等效转化为
```sql
WHERE birth_date <= '1990-01-01' AND points <= 1000
```
### 练习
订单6中总价大于30的商品
```sql
USE sql_store;

SELECT * FROM order_items
WHERE order_id = 6 AND quantity * unit_price > 30
```
注意优先级：数学→比较→逻辑
SELECT 子句，WHERE 子句以及后面的 ORDER BY 子句等都能用列间数学表达式
## 5. IN运算符
<iframe height="450" width="800"  src="//player.bilibili.com/player.html?aid=47123168&bvid=BV1Xb41177na&cid=83131459&page=9" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe> 

### 小结
用IN运算符将某一属性与多个值（一系列值）进行比较
实质是多重相等比较运算条件的简化

### 案例
选出'va'、'fl'、'ga'三个州的顾客
```sql
USE sql_store;

SELECT * FROM customers
WHERE state = 'va' OR state = 'fl' OR state = 'ga'
```
不能 state = 'va' OR 'fl' OR 'ga' 因为数学和比较运算优先于逻辑运算，加括号 state = ('va' OR 'fl' OR 'ga') 也不行，逻辑运算符只能链接布林值。

用 IN 操作符简化该条件
```sql 
WHERE state IN ('va', 'fl', 'ga')
```
可加NOT
```sql
WHERE state NOT IN ('va', 'fl', 'ga')
```
这里可用NOT的原因：可以这么看，IN语句 IN ('va', 'fl', 'ga') 是在进行一种是否符合条件的判断，可看作是一种特殊的比较运算，得到的是一个逻辑值，故可用NOT进行取反
### 练习
库存量刚好为49、38或72的产品
```sql
USE sql_store;

select * from products
where quantity_in_stock in (49, 38, 72)
```

