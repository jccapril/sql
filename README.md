# sql
> Teach Yourself SQL
---------------------
## 检索数据

### 检索单个列
```
SELECT prod_name 
FROM Products;
```
### 检索多个列
```
SELECT prod_id, prod_name, prod_price
FROM Products;
```
### 检索所有列
```
SELECT *
FROM Products;
```
### 检索不同的值
```
SELECT DISTINCT vend_id
FROM Products;
```

### 指定的两列都要完全不同才会检索出来
```
SELECT DISTINCT vend_id, prod_price
FROM Products;
```
### 限制结果
#### 返回不超过5行的数据
```
SELECT prod_name
FROM Products
LIMIT 5;
```
#### 返回从第5行起的5行数据
```
SELECT prod_name
FROM Products
LIMIT 5 OFFSET 5;
```

## 排序检索数据
### 对prod_name按字母顺序排列
```
SELECT prod_name
FROM Products
ORDER BY prod_name;
```
### 按多个列排序
#### 仅当 prod_price 相同时，才会按 prod_name 排序
```
SELECT prod_id, prod_price, prod_name 
FROM Products
ORDER BY prod_price, prod_name;
```
### 按列位置排序
```
SELECT prod_id, prod_price, prod_name 
FROM Products
ORDER BY 2,3
```
### 指定排序方向
#### 按 prod_price 降序排序
```
SELECT prod_id, prod_price, prod_name 
FROM Products
ORDER BY prod_price DESC;
```
#### 按 prod_price 降序排序，再按 prod_name 排序
```
SELECT prod_id, prod_price, prod_name 
FROM Products
ORDER BY prod_price DESC, prod_name;
```

## 过滤数据
### 使用 WHERE 子句
```
SELECT prod_name, prod_price
FROM Products
WHERE prod_price = 3.49;
```
### 范围值检查 BETWEEN
```
SELECT prod_name, prod_price
FROM Products
WHERE prod_price BETWEEN 5 AND 10;
```
### 空值检查
```
SELECT prod_name, prod_price
FROM Products
WHERE prod_price IS NULL;

SELECT cust_name
FROM Customers
WHERE cust_email IS NULL;
```
### 组合 WHERE 子句
```
SELECT prod_id, prod_name, prod_price
FROM Products
WHERE vend_id = 'DLL01' AND prod_price <= 4;

SELECT prod_id, prod_name, prod_price
FROM Products
WHERE vend_id = 'DLL01' or vend_id = 'DLL01';
```
#### AND 优先级比 OR 高
```
SELECT prod_id, prod_name, prod_price
FROM Products
WHERE vend_id = 'DLL01' or vend_id = 'BRS01' AND prod_price >= 10;

SELECT prod_id, prod_name, prod_price
FROM Products
WHERE (vend_id = 'DLL01' or vend_id = 'BRS01') AND prod_price >= 10;
```
### IN 操作符
```
SELECT prod_name, prod_price
FROM Products
WHERE vend_id IN ('DLL01', 'BRS01')
ORDER BY prod_name;
```
#### IN相比OR有什么优点
- IN 操作符语法更加清楚，更直观。
- 在和其他 AND 和 OR 操作符组合使用 IN 时，求值顺序更加容易管理。
- IN 比 OR 执行速度更快。
- IN 最大的优点就是可以包含其他 SELECT 语句， 更够动态地建立 WHERE 子句。

### NOT 操作符
```
SELECT prod_name
FROM Products
WHERE NOT vend_id = "DLL01"
ORDER BY prod_name;
```

## 使用通配符进行过滤
### % 通配符
#### 找到所有以 FISH 开头的 prod_name
```
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE 'FISH%';
```
#### 找到包含 bean bag 的 prod_name 
```
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE '%bean bag%';
```

#### 找到以F开头、以y结尾的 prod_name 
```
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE 'F%y';
```

#### 根据部分信息搜索电子邮件
```
WHERE email LIKE 'b%@qq.com'
```

### _ 通配符
#### _代表一个字符
```
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE '__ inch teddy bear';
```

## 创建计算字段
### 拼接字段
```
SELECT CONCAT(vend_name, ' (', vend_country, ')') AS vend_title
FROM Vendors
ORDER BY vend_name;
```

### 执行算数计算
```
SELECT 	prod_id, 
				quantity, 
				item_price, 
				quantity*item_price AS expanded_price
FROM OrderItems
WHERE order_num = 20008;
```

## 使用函数处理数据
### 文本处理函数
#### UPPER() 将文本转换为大写
```
SELECT vend_name, UPPER(vend_name) as vend_name_upcase
FROM Vendors 
ORDER BY vend_name;
```
#### 常用的文本函数
- LEFT()    返回字符串左边的字符
- RIGTH()   返回字符串右边的字符
- LENGTH()  返回字符串的changed
- UPPER()   将字符串转换为大写
- LOWER()   将字符串转换为小写
- LTRIM()   去掉字符左边的空格
- RTRIM()   去掉字符右边的空格

### 日期和时间处理函数
```
SELECT order_num 
FROM Orders
WHERE YEAR(order_date) = 2012;
```

### 数值处理函数
略

## 汇总数据
### 聚集函数
- AVG()			返回某列的平均数
- COUNT()		返回某列的行数
- MAX()			返回某列的最大值
- MIN()			返回某列的最小值
- SUM()			返回某列之和

### ACG()函数

#### 返回 Products 表中所有产品的平均价格
```
SELECT AVG(prod_price) AS avg_price 
FROM Products
```

####  返回 供应商ID vend_id = DLL01 的产品的平均价格
```
SELECT AVG(prod_price) AS avg_price 
FROM Products
WHERE vend_id = 'DLL01';
```
### COUNT()函数
#### 返回 Customers 表中顾客的总数
```
SELECT COUNT(*) AS num_cust 
FROM Customers;
```
#### 返回具有电子邮件地址的客户总数
```
SELECT COUNT(cust_email) AS num_cust 
FROM Customers;
```
#### COUNT()会忽略指定列的值为空的行，而COUNT(*)不会

### MAX()函数
### 返回 Products 表中最贵的物品价格
```
SELECT MAX(prod_price) as max_price 
FROM Products;
```
### MIN()函数
### 返回 Products 表中最便宜的物品价格
```
SELECT MIN(prod_price) as min_price 
FROM Products;
```
### SUM()函数
#### 返回订单中所有物品数量之和
```
SELECT SUM(quantity) AS items_ordered
FROM OrderItems
WHERE order_num = 20005;
```
#### 返回订单中所有物品价钱之和
```
SELECT SUM(quantity*item_price) AS items_price
FROM OrderItems
WHERE order_num = 20005;
```
### 聚集不同值
#### 上面的表达式输出比下面的低，说明有很多物品具有相同的较低价格。
```
SELECT AVG(prod_price) AS avg_price
FROM Products
WHERE vend_id = 'DLL01';

SELECT AVG(DISTINCT prod_price) AS avg_price
FROM Products
WHERE vend_id = 'DLL01';
```

### 组合聚集函数
```
SELECT	COUNT(*) AS num_items,
				MIN(prod_price) AS price_min,
				MAX(prod_price) AS price_max,
				AVG(prod_price) AS price_avg
FROM Products;
```

## 分组数据
### 按 vend_id 排序并分组数据。
```
SELECT vend_id, COUNT(*) AS num_prods
FROM Products
GROUP BY vend_id;
```
### 过滤分组
#### 过滤COUNT(*)>=2 的分组
```
SELECT cust_id, COUNT(*) AS orders 
FROM Orders
GROUP BY cust_id
HAVING COUNT(*) >= 2;
```
#### WHERE 子句过滤所有 prod_price 至少为 4 的行，然后按 vend_id 分组数据，HAVING 子句过滤计数为 2 或 2 以上的分组
```
SELECT vend_id, COUNT(*) AS num_prods
FROM Products
WHERE prod_price >= 4
GROUP BY vend_id 
HAVING COUNT(*) >= 2;
```

### 分组和排序
```
SELECT order_num, COUNT(*) AS items 
FROM OrderItems
GROUP BY order_num
HAVING COUNT(*) >= 3
ORDER BY items, order_num;
```

### 子查询
```
SELECT cust_id 
FROM Orders
WHERE order_num IN (
	SELECT order_num
	FROM OrderItems
	WHERE prod_id = 'RGAN01'
);
```

```
SELECT cust_name, cust_contact
FROM Customers
WHERE cust_id IN(
	SELECT cust_id 
	FROM Orders
	WHERE order_num IN (
		SELECT order_num 
		FROM OrderItems
		WHERE prod_id = 'RGAN01'
	)
);
```

### 作为计算字段使用自查询
```
SELECT	cust_name,
				cust_state,
				(SELECT COUNT(*) 
					FROM Orders
					WHERE Orders.cust_id = Customers.cust_id) AS orders 
FROM Customers
ORDER BY cust_name;
```


## 联结表
### 创建联结
```
SELECT vend_name, prod_name, prod_price
FROM Vendors, Products
WHERE Vendors.vend_id = Products.vend_id;
```
### 内联结
#### 和上面的语句 其实是一样的
```
SELECT vend_name, prod_name, prod_price
FROM Vendors INNER JOIN Products
ON Vendors.vend_id = Products.vend_id;
```

### 联结多个表
```
SELECT prod_name, vend_name, prod_price, quantity
FROM OrderItems, Products, Vendors
WHERE Products.vend_id = Vendors.vend_id
	AND OrderItems.prod_id = Products.prod_id
	AND order_num = 20007;
```
这个例子显示订单20007的中的物品。该物品存储在 OrderItems 表中。每个产品按其 prod_id 存储，它引用 Products 表中的产品。这个产品通过 vend_id 联结到 Vendors 表中相应的供应商， vend_id 存储在每个产品的记录中。这里的 FROM 子句列出三个表，WHERE 子句定义这两个联结条件，而第三个联结条件用来过滤出订单 20007 中的物品


```
SELECT cust_name, cust_contact 
FROM OrderItems, Customers,  Orders
WHERE Customers.cust_id = Orders.cust_id
	AND Orders.order_num = OrderItems.order_num
	AND prod_id = 'RGAN01'
```
和 自查询第二个例子表达的意思相同，但是最有效的方法应该用联结

## 高级联结