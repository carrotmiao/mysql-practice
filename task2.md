#task02 by 萝卜苗
##2.1
```sql
#查找
SELECT product_name,regist_date 
FROM product
WHERE regist_date >'2009-04-28';
```
结果见图2.1
![2.1](2.1.PNG "2.1.png")
##2.2
对于
```sql
SELECT *
  FROM product
 WHERE purchase_price = NULL;
 ```
 得到的结果如下
![2.2.1](2.2.1.PNG)
对于
```sql
SELECT *
  FROM product
 WHERE purchase_price <> NULL;
 ```
 得到的结果如下
 ![2.2.2](2.2.2.PNG)
 对于
 ```sql
 SELECT *
  FROM product
 WHERE product_name > NULL;
 ```
 私以为NULL无法与字符串作比较，所以无法输出结果。
##2.3
我只能想到一条
```sql
--2.3方法一
SELECT product_name,sale_price,purchase_price
FROM product 
WHERE sale_price -purchase_price >=500;
```
##2.4
代码如下
```sql
SELECT product_name, product_type,sale_price *0.9-purchase_price AS profit
FROM product
WHERE (product_type='办公用品'
   OR product_type ='厨房用具')
  AND sale_price *0.9-purchase_price>100;
```
得到的结果为
![2.4](2.4.PNG)

##2.5
对于
```sql
SELECT product_id, SUM（product_name）
--本SELECT语句中存在错误。
  FROM product 
 GROUP BY product_type 
 WHERE regist_date > '2009-09-01';
```
发现了以下错误：
1.groupby的内容没有出现在select中
2.where不能放在group by之后
3.SUM只能对数值列操作
##2.6
```sql
 SELECT product_type, sum(sale_price), sum(purchase_price)
 FROM product
 GROUP BY product_type 
 HAVING sum(sale_price)>1.5*sum(purchase_price); 
```
执行结果如下
![2.6](2.6.PNG)
##2.7
首先发现regist_date是按照降序排列的，但是NULL放在最上面；再比较相同regist_date发现sale_price是升序排列的，于是以regist_date作为主排序基准，sale_price作为次排序基准来进行排序，代码如下：
```sql
SELECT *
FROM product
ORDER BY -regist_date,sale_price; 
```
得到结果如下
![2.7](2.7.PNG)

##一点笔记
###2.4.1 聚合函数
SQL中用于汇总的函数叫做聚合函数。以下五个是最常用的聚合函数：
- SUM：计算表中某数值列中的合计值
- AVG：计算表中某数值列中的平均值
- MAX：计算表中任意列中数据的最大值，包括文本类型和数字类型
- MIN：计算表中任意列中数据的最小值，包括文本类型和数字类型
- COUNT：计算表中的记录条数（行数）
###GROUP BY书写位置
GROUP BY的子句书写顺序有严格要求，不按要求会导致SQL无法正常执行，目前出现过的子句顺序为：

SELECT ➡️ 2. FROM ➡️ 3. WHERE ➡️ 4. GROUP BY
其中前三项用于筛选数据，GROUP BY对筛选出的数据进行处理