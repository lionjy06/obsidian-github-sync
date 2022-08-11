기본적인 형태의 JOIN은 inner join으로 가져오려는 컬럼값에 NULL이 있으면 그것을 가져오지 않는다. 

하지만 outer조인은 조인하려는 테이블에 NULL이 존재하더라도 그냥 일단 가져오고 본다. 

outer join에는 left와 right조인 이 있는데 진짜 별거아닌 컨셉이다.

```
SELECT 
	C.CustomerName, S.SupplierName, C.City, C.Country 
FROM 
	Customers C 
LEFT JOIN 
	Suppliers S 
ON 
	C.City = S.City AND C.Country = S.Country; -- LEFT를 RIGHT로 바꿔서도 실행해 볼 것
```

left조인을 예로들면 C가 왼쪽에 있다 즉 customer 테이블에 City컬럼값중 null인게 있어도 s가 null이 아니라면 그냥 그 값을 category로 가져온다.

right일 경우는 반대로 Supplier 테이블에 country값이 null이어도 customer테이블에 있다면 가져온다