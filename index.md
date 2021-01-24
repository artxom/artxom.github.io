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