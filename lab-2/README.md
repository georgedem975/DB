__Примеры запросов с решениями__
1. Найти номера первых трех подкатегорий (ProductSubcategoryID) с наибольшим количеством наименований товаров.

```sql
SELECT TOP WITH TIES 3 [ProductSubcategoryID]
FROM [Production].[Product]
WHERE [ProductSubcategoryID] IS NOT NULL
GROUP BY [ProductSubcategoryID]
ORDER BY COUNT(*) DESC
```

2. Разбить продукты по количеству символов в названии, для каждой группы определить количество продуктов.

```sql
SELECT LEN([Name]), COUNT(*)
FROM [Production].[Product]
GROUP BY LEN([Name])
```

3. Проверить, есть ли продукты с одинаковым названием, если есть, то вывести эти названия.

```sql
SELECT [Name]
FROM [Production].[Product]
GROUP BY [ProductID], [Name]
HAVING COUNT(*)>1
```

__Задания для самостоятельной работы__
1. Найти и вывести на экран количество товаров каждого цвета, исключив из
поиска товары, цена которых меньше 30.

```sql
SELECT p.Color, count(*)
FROM Production.Product as p
WHERE p.ListPrice >= 30
GROUP BY p.Color
```

2. Найти и вывести на экран список, состоящий из цветов товаров, таких, что
минимальная цена товара данного цвета более 100.

```sql
SELECT p.Color, count(*)
FROM Production.Product as p
GROUP BY p.Color
HAVING MIN(p.ListPrice) > 100
```

3. Найти и вывести на экран номера подкатегорий товаров и количество товаров
в каждой категории.

```sql
SELECT p.ProductSubcategoryID, count(*)
From Production.Product as p
GROUP BY p.ProductSubcategoryID
```

4. Найти и вывести на экран номера товаров и количество фактов продаж данного товара (используется таблица SalesORDERDetail).

```sql
SELECT s.ProductID, count(*)
FROM Sales.SalesOrderDetail as s
GROUP BY s.ProductID
```

5. Найти и вывести на экран номера товаров, которые были куплены более пяти
раз.

```sql
SELECT s.SalesOrderID, count(*)
FROM Sales.SalesOrderDetail as s
GROUP BY s.SalesOrderID
HAVING count(*)>5
```

6. Найти и вывести на экран номера покупателей, CustomerID, у которых
существует более одного чека, SalesORDERID, с одинаковой датой

```sql
select p.CustomerID
from Sales.SalesOrderHeader as p
group by p.CustomerID, p.OrderDate
having count(*) > 1
```

7. Найти и вывести на экран все номера чеков, на которые приходится более трех продуктов.

```sql
SELECT s.SalesOrderID, count(ProductID) as prod
FROM Sales.SalesOrderDetail as s
GROUP BY s.SalesOrderID
HAVING count(ProductID)>3
```

8. Найти и вывести на экран все номера продуктов, которые были куплены более
трех раз.

```sql
SELECT s.ProductID, count(ProductID) as prod
FROM Sales.SalesOrderDetail As s
GROUP BY s.ProductID
HAVING count(ProductID) > 3
```

9. Найти и вывести на экран все номера продуктов, которые были куплены или
три или пять раз.

```sql
SELECT s.ProductID, count(ProductID) as prod
FROM Sales.SalesOrderDetail As s
GROUP BY s.ProductID
HAVING count(ProductID)=3 OR count(ProductID)=5
```
10. Найти и вывести на экран все номера подкатегорий, в которым относится
более десяти товаров.

```sql
SELECT p.ProductSubcategoryID, count(*)
FROM Production.Product as p
WHERE p.ProductSubcategoryID is not null
GROUP BY p.ProductSubcategoryID
HAVING count(*) > 10
```

11. Найти и вывести на экран номера товаров, которые всегда покупались в
одном экземпляре за одну покупку.

```sql
SELECT p.ProductID
FROM Sales.SalesOrderDetail as p
GROUP BY p.ProductID, p.OrderQty
HAVING 1=ALL(SELECT p.OrderQty)
```

12. Найти и вывести на экран номер чека, SalesORDERID, на который приходится
с наибольшим разнообразием товаров купленных на этот чек.

```sql
SELECT TOP 1 s.SalesOrderID, count(DISTINCT s.ProductID)
FROM Sales.SalesOrderDetail as s
GROUP BY s.SalesOrderID
```

13. Найти и вывести на экран номер чека, SalesORDERID с наибольшей суммой
покупки, исходя из того, что цена товара – это UnitPrice, а количество
конкретного товара в чеке – это ORDERQty.

```sql
SELECT TOP 1 s.SalesOrderID, sum(s.UnitPrice * s.OrderQty) as sum
FROM Sales.SalesOrderDetail as s
GROUP BY s.SalesOrderID
ORDER BY sum DESC
```

14. Определить количество товаров в каждой подкатегории, исключая товары,
для которых подкатегория не определена, и товары, у которых не определен цвет.

```sql
SELECT p.ProductSubcategoryID, count(p.ProductID) as count
FROM Production.Product as p
WHERE p.ProductSubcategoryID is not null and p.Color is not null
GROUP BY p.ProductSubcategoryID
```

15. Получить список цветов товаров в порядке убывания количества товаров данного цвета

```sql
SELECT p.Color, count(*) as count
FROM PRoduction.Product as p
WHERE p.Color is not null
GROUP BY p.Color
ORDER BY count DESC
```

16. Вывести на экран ProductID тех товаров, что всегда покупались в количестве более 1 единицы на один чек, при этом таких покупок было более двух.

```sql
SELECT p.ProductID, count(*)
FROM Sales.SalesOrderDetail as p
WHERE p.OrderQty > 1
GROUP BY p.ProductID
HAVING count(*) > 2
```