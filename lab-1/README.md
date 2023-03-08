__Примеры запросов с решениями__
1. Получить все названия товаров из таблицы Product.

```sql
SELECT p.Name
FROM [Production].[Product] AS p
```
В данном случае для таблицы Product из схемы Production введен псевдоним p.
Разумное использование псевдонимов позволяет увеличить скорость написания
кода в MS SQL Management Studio.

2. Получить все названия товаров в системе, цена которых (listprice) выше 200.

```sql
SELECT p.Name
FROM [Production].[Product] AS p
WHERE p.ListPrice>200
```

3. Получить все названия товаров в системе цена которых (listprice) выше 200 и у которых первая буква в названии “S”.

```sql
SELECT p.Name
FROM [Production].[Product] AS p
WHERE p.ListPrice>200 AND p.Name like 's%'
```

4. Получить все названия товаров в системе, цена которых (listprice) выше 200 и у которых в названии есть сочетание символов “are”.

```sql
SELECT p.Name
FROM [Production].[Product] AS p
WHERE p.ListPrice>200 AND p.Name like '%are%'
```

5. Получить все названия товаров в системе, в названии которых третий символ
– либо буква “s”, либо буква “r”. Решить задачу как минимум двумя способами.

```sql
SELECT p.Name
FROM [Production].[Product] AS p
WHERE p.Name like '__s%' or p.Name like '__r%'
```
<p style="text-align: center">или</p>

```sql
SELECT p.Name
FROM [Production].[Product] AS p
WHERE p.Name like '__[sr]%'
```

7. Получить все названия товаров в системе, в названии которых ровно 5 символов.

```sql
SELECT p.Name
FROM [Production].[Product] AS p
WHERE p.Name like '_____'
```

<p style="text-align: center">Или второй вариант</p>

```sql
SELECT p.Name
FROM [Production].[Product] AS p
WHERE len(p.Name)=5
```

8. Найти, без повторений, номера товаров, которые были куплены хотя бы один раз.

```sql
SELECT DISTINCT sod.ProductID
FROM [Sales].[SalesORDERDetail] AS sod
```

9. Найти список всех возможных стилей (style) продукта, без повторений.

```sql
SELECT DISTINCT p.Style
FROM [Production].[Product] AS p
WHERE p.Style is NOT null
```

10.Написать запрос, который возвращает названия товаров, которые были
произведены между мартом 2011 года и мартом 2012 года включительнo (необходимо учитывать формат даты)

```sql
SELECT p.Name
FROM Production.Product AS p
WHERE CAST(p.SellStartDate AS DATE)>='2011-03-01'
AND CAST(p.SellStartDate AS DATE)<='2012-03-31'
```

11. Найти максимальную стоимость товара (отпускная цена ListPrice) из тех, которые были произведены, начиная с марта 2011 года.

```sql
SELECT max(p.ListPrice) AS [max price]
FROM [Production].[Product] AS p
WHERE p.SellStartDate>='2011-01-03'
```

12. Вывести название и цвет продукта, отсортированные по названию продукта по возрастанию.

```sql
SELECT p.Name, p.Color
FROM [Production].[Product] AS p
ORDER BY p.Name ASc
```

13. Вывести названия продукта, цвет и отпускную цену для таких товаров, у
которых цвет определен и цена отлична от нуля, и отсортировать полученный
список по возрастанию цвета товара и убыванию отпускной цены.

```sql
SELECT p.Name, p.Color, p.ListPrice
FROM [Production].[Product] AS p
WHERE p.Color is NOT null AND p.ListPrice!=0
ORDER BY p.Color, p.ListPrice desc
```

14. Получить название продукта и разницу между отпускной стандартной ценой
продукта и стандартной ценой продукта для тех товаров, у которых эти
показатели не равны нулю.

```sql
SELECT p.Name, p.ListPrice-p.StandardCost
FROM [Production].[Product] AS p
WHERE p.ListPrice!=0 AND p.StandardCost!=0
```

15. Найти название самого дорогого товара, исходя из предположения, что нет
двух товаров с одинаковой ценой.

```sql
SELECT top 1 with ties p.Name
FROM [Production].[Product] AS p
ORDER BY p.ListPrice desc
```

16. Найти список товаров, произведенных в 2005 году.

```sql
SELECT p.Name
FROM [Production].[Product] AS p
WHERE datepart(YEAR,p.SellStartDate)=2005
```

__Задания для самостоятельной работы__
1. Найти и вывести на экран названия продуктов, их цвет и размер.

```sql
SELECT p.Name, p.Color, p.Size
FROM Production.Product AS p
```

2. Найти и вывести на экран названия, цвет и размер таких продуктов, у которых цена более 100.

```sql
SELECT p.Name, p.Color, p.Size
FROM Production.Product AS p
WHERE p.ListPrice > 100
```

3. Найти и вывести на экран название, цвет и размер таких продуктов, у которых цена менее 100 и цвет Black.

```sql
SELECT p.Name, p.Color, p.Size
FROM Production.Product AS p
WHERE p.ListPrice < 100 AND p.Color='black'
```

4. Найти и вывести на экран название, цвет и размер таких продуктов, у которых цена менее 100 и цвет Black, упорядочив вывод по возрастанию стоимости продуктов.

```sql
SELECT p.Name, p.Color, p.Size
FROM Production.Product AS p
WHERE p.ListPrice < 100 AND p.Color='black'
ORDER BY p.ListPrice ASC
```

5. Найти и вывести на экран название и размер первых трех самых дорогих
товаров с цветом Black.

```sql
SELECT top 3 p.Name, p.Color
FROM Production.Product AS p
WHERE p.Color='black'
ORDER BY p.ListPrice DESC
```

6. Найти и вывести на экран название и цвет таких продуктов, для которых
определен и цвет, и размер.

```sql
SELECT p.Name, p.Color
FROM Production.Product AS p
WHERE p.Color is not null and p.Size is not null
```

7. Найти и вывести на экран не повторяющиеся цвета продуктов, у которых цена
находится в диапазоне от 10 до 50 включительно.

```sql
SELECT p.Name, p.ListPrice
FROM Production.Product AS p
WHERE p.ListPrice BETWEEN 10 and 50
```

8. Найти и вывести на экран все цвета таких продуктов, у которых в имени первая буква ‘L’ и третья ‘N’.

```sql
SELECT p.Color
FROM Production.Product AS p
WHERE p.Name like 'l_n%'
```

9. Найти и вывести на экран названия таких продуктов, которых начинаются
либо на букву ‘D’, либо на букву ‘M’, и при этом длина имени – более трех
символов.

```sql
SELECT p.Name
FROM Production.Product AS p
WHERE p.Name like 'd___%' or p.Name like 'm___%'
```

10. Вывести на экран названия продуктов, у которых дата начала продаж – не
позднее 2012 года.

```sql
SELECT p.Name
FROM Production.Product AS p
WHERE CAST(p.SellStartDate AS DATE) <= '2012.12.31'
```

11. Найти и вывести на экран названия всех подкатегорий товаров.

```sql
SELECT p.Name
FROM Production.ProductSubcategory AS p
```

12. Найти и вывести на экран названия всех категорий товаров.

```sql
SELECT p.Name
FROM Production.ProductCategory AS p
```

13. Найти и вывести на экран имена всех клиентов из таблицы Person, у которых обращение (Title) указано как «Mr.».

```sql
SELECT p.FirstName
FROM Person.Person AS p
WHERE p.Title like 'Mr.'
```

14. Найти и вывести на экран имена всех клиентов из таблицы Person, для
которых не определено обращение (Title).

```sql
SELECT p.FirstName
FROM Person.Person AS p
WHERE p.Title is null
```