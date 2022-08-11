## GROUP BY

### group by 는 어떤 특정 컬럼값으로 묶어 중복되는 값을 잡아준다. 
*  이떄 중요한점은 group by는 중복값은 잡아주나 그 값에 대한 집계는 가능하다는 점이다. 그래서 집계함수와 사용이되기도 한다.*

사용법
```
SELECT <컬럼>, <컬럼>
FROM <테이블>
GROUP BY <컬럼>
```

예시
```
SELECT Country 
FROM Customers 
GROUP BY Country;
```

### Group by사용시 꼭 조합으로 사용하는것도 가능하다. 
```
SELECT 
	Country, 
	City, 
	CONCAT_WS(', ', City, Country) 
FROM Customers 
GROUP BY Country, City;
```

이럴경우 country와 city의 조합으로 묶여 같은 조합이 있을시 하나의 조합만 리턴해준다.

### 집계함수와 함께 사용되는 Group by
```
SELECT
  COUNT(*), OrderDate
FROM Orders
GROUP BY OrderDate;
```
이럴경우 orderDate의 컬럼값을 집계하여 중복값은 없에되 몇개 중복됫는지는 파악할수있다.

```
SELECT
  ProductID,
  SUM(Quantity) AS QuantitySum
FROM OrderDetails
GROUP BY ProductID
ORDER BY QuantitySum DESC;
```
이 경우에는 ProductID에 중복되는 값들은 잡아주되 SUM으로 각 ProductID에 해당하는 Quantity들의 총합을 리턴한다.


### WITH ROLLUP
각 컬럼값들의 총합을 집계한 수치를 보여주는데 만약 이걸 쓸거면 order by를 사용할수 없다는 단점이 있다.

```
SELECT 
	Country, 
	COUNT(*) 
FROM Suppliers 
GROUP BY Country 
WITH ROLLUP;
```


### HAVING
HAVING의 경우는 order by와 비슷한데 order by는 필터링의 역할을 수행하며 쿼리의 결과물이 나오기전 어떤 값들을 가지고 쿼리를 돌릴지 작업전 필터링이라면 HAVING은 쿼리가 완료된 후 작업이 완료되고 필터링을 걸수있다.

```
SELECT 
	CategoryID, 
	MAX(Price) AS MaxPrice, 
	MIN(Price) AS MinPrice, 
	TRUNCATE((MAX(Price) + MIN(Price)) / 2, 2) AS MedianPrice, 
	TRUNCATE(AVG(Price), 2) AS AveragePrice 	
FROM 
	Products 
WHERE 
	CategoryID > 2 
GROUP 
	BY CategoryID 
HAVING
	AveragePrice BETWEEN 20 AND 30 AND MedianPrice < 40;
```

이떄의 HAVING은 쿼리작업후 나온 결과물을 필터링하여 값을 출력한다.


### DISTINCT
역할은 group by랑 비슷하게 중복값을 잡아주는 역할을 하지만 그러면서 집계를 할순없다. 그것이 단점이지만 장점은 group by 보다 쿼리속도가 훨씐빠르다 그래서 단순 중복값 잡아주는 쿼리로는 이게 더 낫다는 평도 있다.

```
SELECT 
	Country, 
	COUNT(DISTINCT CITY) 
FROM 
	Customers 
GROUP BY 
	Country;
```
DISTINCT city를 통해 city의 중복값을 지우고  group by를 통해 각 해당 Country안에는 몇개의 City가 있는지 알수있다(원래는 어떤 City가 있는지 뜨겠지만 Count와 DISTINCT로 잡아줌)