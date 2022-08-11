
## SELECT
sql을 공부하며 가장 기본이 되는 기능중 하나지만 이용하기에 따라 활용가능성이 무궁무진한 친구이다. 일단 select에서는 지정된 값을 가져올수 있다. 만약 내가 Products라는 테이블이 있고 그안에 id, name, description, price, category라는 칼럼값이 있다고 가정해보자. 그럼 Products에서 모든 컬럼값을 가져올수도, 내가 가져오고 싶은 컬럼만 가져올수있다. 그럼 여기서 몇가지 예시를 보여주도록 해보겠다.

```
1. SELECT * FROM Products

2. SELECT price, category FROM Products

```

1번 예제에서는 Products안에든 모든 컬럼들을 가져올것이고 2번 예제에서는 price와 category값만 가져올수 있다. 물론 이것말고도 할수있는 것은 너무많지만 일단은 그건 추후에 조금씩 더 부가기능을 추가해보도록 해보겠다.

## WHERE
where은 select에서 가져온 컬럼값들을 필터링 해주는 기능을 한다. 예를 들면 price를 가져왔는데 5000원 이상인것만 가져올수도 있고, category들을 전부 가져왔는데 그중에 '운동'이라는 값만 가져오고 싶을때와 같이 가져온 컬럼값들을 필터링 할수있다.

```
1. SELECT price, category FROM Products WHERE price >= 5000;

2. SELECT * FROM Products WHERE category = '운동'; 
```


## ORDER BY
order by는 select로 가져온 값을 정렬해 주는 기능이다. order by는 기본적으로 ASC(오름차순)을 기본으로 하여 아무것도 정해주지 않는다면 ASC가 적용되지만 DESC를 따로 지정해주면 내림차순으로 나온다.

```
1. SELECT price, category FROM Products ORDER BY price

2. SELECT * FROM Products ORDER BY price DESC;

3. SELECT * FROM Products ORDER BY price ASC
```
1, 3 예제는 둘다 price값의 오름차순으로 낮은것 부터 큰것 순으로 정렬이 되지만 2번은 내림차순으로 정렬되어 높은것 부터 낮은순으로 보여진다.

## AS
AS는 SELECT로 가져오는 값들을 내가 부르기 편한 이름으로 변경할수있다. 
```
SELECT Price AS P, category AS cate FROM Products 
```
이렇게 하면 price는 p라는 이름의 컬럼으로 category는 cate라는 이름의 컬럼으로 변경된다.


## LIMIT
limit은 우리가 흔히 알고 있는 limit 기능도 하며 offset기능도 함께 할수있다. 
```
1. SELECT * FROM Products LIMIT 10;

2. SELECT * FROM PRoducts LIMIT 0, 10;
```

1번 예제는 말 그대로 받아올수 있는 데이터의 갯수를 정해놓은것이다. 하지만 2번은 페이지네이션에 필요한 offset기능을 한다.

오늘의 sql 정리는 여기까지 하겠다. 내 공부 하려고 작성하는 것이지만 다른사람도 보고 쉽게 이해할수있도록 썻기 때문에 응용보다는 기본에 충실하게 정리했다.