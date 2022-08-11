## 사칙연산

사칙연산이라 함은 더하기, 뺴기, 나누기, 곱하기를 의미한다. 하지만 우리는 여기서 하나 더 나아가 나머지 까지 알아보도록 하자.

```null
SELECT
1 + 1,	//2
2 - 1,	//1
2 * (1 + 2), //6
9 / 3,	//3
10 % 3 	//1
```

맨위부터 보자면 더하기 빼기 곱하기 나누기 그리고 나머지 이다. 너무 쉬우니 별다른 설명은 패스

```null
SELECT 
'abc' + 3,	//3
'abc' * 3,	//0
'1' + '002' * 3	//7
```

이건 좀 신기했다. 우선 'abc'는 단순 문자열이고 3은 숫자다. 이 두개를 더하면 js를 공부했던 나는 당연하 'abc3'이 될줄알았는데 왠걸 3이 반환된다. 그리고 'abc' _3 은 0이 된다. 이 결과로 미루어봤을때 앞이 단순 문자열이라도 뒤에 연산자가 붙고 숫자가 붙는다면 'abc' = 0 으로 인식된다는 결과를 도출해낼수 있을것이다. 그리고 '1' + '002'_ 3은 7이 나왔다. 문자열 형태로 된 숫자라도 연산자가 붙으면 그냥 숫자가 된다는 것을 알수있었다.

## 참거짓

참거짓은 말그대로 true(1) false(0)을 가리는 연산이다. 여기서 사용되는 몇가지 연산자가 있다.

```null
SELECT 
NOT TRUE, //0
NOT FALSE,		//1
TRUE, 	//1
FALSE,	//0
1 IS 1,	//1
1 IS NOT 1	//0
```

일단 NOT부터 설명하자면 반대의 수식어다. true의 반대니 false그래서 0, 그리고 IS는 = 와 비슷하다.  
(1 Is 1) = (1 = 1) 등호를 뜻하며 이경우는 참이기 때문에 1, IS NOT은 !=를 뜻한다.

```null
SELECT
1 IS 1 AND TRUE,
1 IS 2 AND TRUE,
1 IS 1 & TRUE,
1 IS NOT 1 OR TRUE,
1 IS 1 OR FALSE,
1 IS 1 || FALSE
```

a and b 에서 1(true)를 리턴하려면 a 와 b 모두 true여야 한다. 따라서 첫번째는 둘다 트루이기 때문에 1을 리턴한다.

a or b 에서 1(true)를 리턴하려면 a 혹은 b 둘중하나만 true여도 true를 리턴한다.

참고로 AND = & 그리고 OR = || 이다.

```null
1. 
SELECT 
*
FROM [Products]
WHERE
SupplierID BETWEEN 1 AND 3

2.
SELECT 
*
FROM [Products]
WHERE
SupplierID NOT BETWEEN 1 AND 3
```

1번 예제에 between a and b는 where에 필터링 조건으로 들어간다. 즉 supplierID중 1~4사이의 값만 가져오고 2번 예제는 반대로 1~4를 제외한 값들을 가져온다.

```null
1. 
SELECT
SupplierID IN (1,2,3)
FROM Products

2.
SELECT
SupplierID NOT IN (1,2,3)
FROM Products
```

1번 예제의 경우 supplierID가 괄호안의 값중 참인값이 있다면 1(true)를 아니면 0(false)을 리턴한다.

2번은 supplierID가 괄호안에 없으면 1(true)를 있다면 0(false)를 리턴한다.

```null
SELECT 
* 
FROM [Categories]
WHERE CategoryName LIKE '%dim%'
```

LIKE '&' 와 '_'가 있다.  
LIKE '&'는 글자수의 상관없이 조건에 맞는 값을 가져온다  
LIKE 'a%' => a로 시작하는 문자열을 다가져옴(글자수 상관x)  
LIKE '%a' => a로 끝나는 문자열을 다가져옴(글자수 상관x)  
LIKE '%a% => a가 포함된 문자열을 다가져옴글자수 상관x)

LIKE '_'는 글자수를 지정할수있고 그 글자수 만큼의 조건을 만족하는 문자열만 가저옴  
LIKE '**_a' a로 끝나며 앞에 3개의 문자가 오는 문자열만 가져옴  
LIKE 'a_**' a로 시작하며 뒤에 3개의 문자가 오는 문자열만 가져옴  
LIKE '**_a_**' a가 가운데 있고 앞뒤 세개씩의 문자가 오는 문자열만 가저옴

사실 기본적이라고는 하지만 활용도에 따라 무궁무진하게 활용할수있는 길이 많기 때문에 프로그래밍도 마찬가지고 sql도 상상력이 좋은사람이 활용을 잘하는 사람이라고 생각된다.
