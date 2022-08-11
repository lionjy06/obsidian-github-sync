## 비상관 서브쿼리
### 본쿼리와 서브쿼리와의 상관관계가 독립적인 쿼리형태

서브 쿼리란 쿼리안에서 존재하는 부분 쿼리같은 느낌으로 알면 편하다.

첫번쨰로 알아볼것은 비상관 서브쿼리인데 사실 이건 별쓸모가 없다, 왜냐하면 지금 내가 해야하는 작업(쿼리)와 아무 관련없는 쿼리가 실행될것이기 때문이다. 우리는 작업을 할때 어떠한 쿼리도 불필요한 쿼리가 있어서는 안됀다. 불필요한 쿼리가 끼는 순간 서버의 성능 부하가 불필요하게 심해지기 때문이다.

```
SELECT 
	CategoryID, 
	CategoryName, 
	Description, 
	(SELECT ProductName FROM Products WHERE ProductID = 1) 
FROM Categories;
```
현재 가져오는 테이블은 Category 인데 잘보면 서브쿼리로 가져오는것들은 전혀 무관한 productName 같은것들이다. 물론 이것들이 연결되어 있다면 혹은 where절로 어떠한 의미를 갖고 쿼리에도 내가 원하는 방향의 작업을 이끌어 낼수 있다면 좋겠지만 이거 자체만으로는 아무짝에도 쓸때가 없는 서브쿼리일 뿐이다.


이번에 볼것은 어느정도 쿼리에 유의미한 결과를 가져오는 서브쿼리문을 만들어 보겠다.

```
SELECT 
	* 
FROM 
	Products 
WHERE 
	Price < ( SELECT AVG(Price) FROM Products );
```

이건 의미가 있는 서브쿼리이다. 왜냐하면 이 서브쿼리는 where절에 걸려 price가 평균보다 낮은 것을 찾아올수있게 해주는 서브쿼리 이기 때문이다.

```
SELECT 
	CategoryID, 
	CategoryName, 
	Description 
FROM 
	Categories 
WHERE 
	CategoryID = (SELECT CategoryID FROM Products WHERE ProductName = 'Chais');
```

이것 또한 유의미한 서브쿼리라고 볼수있다. 이 예시는 조인된 테이블에값에서 공유하는 ID값을 가져와 해당 아이디 값에 일치하는 raw에서 SELECT로 지정한 컬럼값들을 가져오기 때문이다.

category id,name,description을 가져오되 product테이블에서 공유되고있는 CategoryID 중 ProductName이 'Chais'인것을 찾아서 그 CateogoryID에 해당하는 id를 where 절에 넣어 Cateogry테이블로 부터 값을 가져오는것

조금 설명이 어렵겠지만 알고보면 그렇게 어려운건 아니라 이해만 좀 할수있으면 될것같다.

```
SELECT 
	CategoryID, 
	CategoryName, 
	Description 
FROM 
	Categories 
WHERE 
	CategoryID IN (SELECT CategoryID FROM Products WHERE Price > 50);
```
서브 쿼리가 where절에 존재하는데, Products 테이블에서 price가 50 이상인 CateogryID를 가져와서 본쿼리에 존재하는 CategoryID들과 비교(IN)하여 조건에 맞는(price > 50) 이상의 CategoryID를 가지고 있는 id,name,description을 가져오게 한다.


### All, Any
All은 무엇보다도 높은 또는 낮은(조건에 따라 다름) , any는 조건에 맞기만하면 o
```
SELECT 
	*
FROM 
	Products 
WHERE 
	Price > ALL ( SELECT Price FROM Products WHERE CategoryID = 2 );
```
categoryID가 2인 제품중 그 어떤것보다 비싼 값(raw)들을 가져와라

```
SELECT 
	CategoryID, 
	CategoryName, 
	Description 
FROM 
	Categories 
WHERE 
	CategoryID = ANY (SELECT CategoryID FROM Products WHERE Price > 50);
```
price가 50인 categoryID 아무거나랑 조건이 맞기만하면됨


