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
