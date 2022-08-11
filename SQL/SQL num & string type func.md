## 숫자 관련 함수
숫자 관련 함수
```
SELECT
ROUND(Price),	//price의 반올림
CEIL(Price),	//price의 올림
FLOOR(Price),	//price의 내림
GREATEST(OrderID,ProductId,QuantityID),	//3개의 컬럼중 가장 큰값
LEAST(OrderID,ProductId,QuantityID),	//3개의 컬럼중 가장 작은값
MAX(Price),	//price에서 가장큰값
MIN(Price),	//price에서 가장 작은값
COUNT(Price),	//price컬럼의 갯수
SUM(Price),	//price값들의 총합
AVG(Price),	//price값들의 평균
ABS(-5),	//5 => abs는 절대값
POW(2,3),	// 8 2의3제곱
SQRT(16,2),	//4 16의 제곱근
TRUNCATE(1234.1234,0)	//1234 소숫점없는 값
TRUNCATE(1234.1234,1)	//1234.1 소숫점 첫번쨰까지
TRUNCATE(1234.1234,2)	//1234.12 소숫점 두번째까지
TRUNCATE(1234.1234,-1)	//1230 10의자리까지
TRUNCATE(1234.1234,2)	// 1200 100의자리 까지
```


문자 관련 함수
```
SELECT
UPPER('abcDEF')	//ABCDEF
LOWER('abcDEF')	//abcdef
CONCAT('abc','','def')	//abc def
CONCAT_WS('-',2022,8,7)	//2022-8-7
SUBSTR('ABCDEFG', 3),	//CDEFG	(인자가 하나만 있으면 그 인자 부터 끝까지)
SUBSTR('ABCDEFG', 3, 2),	//CD	(인자가 두개있으면 그 인자부터 두번째 인자의 갯수만큼)
SUBSTR('ABCDEFG', -4),	//DEFG (인자가 - 면 뒤에서부터 카운트)
SUBSTR('ABCDEFG', -4, 2);	//DE (뒤에서부터 4번째 부터 2개)
LEFT('abcdef',3),	//abc 왼쪽에서 부터 3개
RIGHT('abcdef',3),	//def 오른쪽에서 부터 3개
LENGTH('abcdef')	//글자의 바이트 갯수
CHAR_LENGTH('abcdef'),	//글자의 갯수 => 우리가 흔히 생각하는 글자갯수는 이거다
TRIM('  abcdef    '),	//abcdef
LTRIM(' abcdef  '),	//abcdef...
RTRIM(' abcdef  '),	//...abcdef
LPAD('abc',5,'-'),	//--abc	첫번째인자에서 두번째 인자 만큼 글자수가 찰떄까지 세번째 인자를 왼쪽에서 부터 체움

RPAD('abc',5,'-'),	//abc-- 첫번째인자에서 두번째 인자 만큼 글자수가 찰떄까지 세번째 인자를 오른쪽에서 부터 체움

REPLACE('안녕하세요 저는 brad입니다.','brad','jinyoung'), 첫번쨰 인자의 문자열에서 두번째 인자를 세번째 인자로 바꿈

INSTR('abcdef','abc'), //1 두번째 인자와 첫번째 인자를 매칭했을때 두번째 인자의 시작점과 첫번째의 자리가 같은곳 (없다면 0을 리턴함)

INSTR('abcdef','def'),	//4 두번째 인자와 첫번째 인자를 매칭했을때 두번째 인자의 시작점과 첫번째의 자리가 같은곳 (없다면 0을 리턴함)

CAST('01',DECIMAL), 첫번째 인자를 두번쨰인자의 형태로 바꿈
CONVERT('1',DECIMAL)	첫번째 인자를 두번쨰인자의 형태로 바꿈
```

함수는 기능이고 외우는거라 이해해야 되는 부분이 적어 설명도 별로없었다...
 
