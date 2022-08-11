## DATE / TIME

오늘은 sql에서 지원하는 날짜, 시간 등에 대한 함수를 알아보고자 한다.

우선 현재의 시간/날짜로는 current_date(), curdate()는 현재의 날짜를 리턴한다. 그리고 current_time, curtime()은 현재의 시간을 리턴한다. 마지막으로는 current_timestamp, now()는 현재의 날짜와 시간을 리턴한다.

date('날짜') => 날짜형으로 바뀜 ex date('2022-08-09')
time('시간') => 시간형으로 바뀜 ex time('1:2:3') = 1시2분3초

그 외에도 년,월,일,날짜,요일 등에대한 함수도 존재한다.
![](https://velog.velcdn.com/images/lionjy06/post/bb404a41-6311-44a2-bfbf-8d6e2f7da37d/image.png)

이 식에 대한 결과로는 아래와 같을수 있다.
![](https://velog.velcdn.com/images/lionjy06/post/6c5606ce-85fb-4266-a2d2-efa3465add7b/image.png)

date_diff, time_diff

DATEDIFF는 날짜간의 차이를 리턴한다 date_diff('날짜1','날짜2') 즉 여기서는 날짜1 - 날짜2 라고 보면 쉬울것이다.

비슷하게 TIMEDIFF('시간1' - '시간2') 즉 여기서는 시간1 - 시간2로 계산되서 나온다.

DATE_FORMATE('날짜',날짜 포맷)
DATE_FORMAT은 원하는 날짜를 원하는 포맷에 지정하여 보여준다.

%y연, %m월, %D(1st, 2nd, 3rd... 일을 영문으로), %d(%e)일, %T시간(hh:mm:ss), %H(%k)tl(0~23), %h(%l)시(0~12), %i분, %S(%s)초, %p am/pm   

![](https://velog.velcdn.com/images/lionjy06/post/3c8873ff-4160-4f7d-be27-119271e7b087/image.png)

이런식으로 포맷팅을 할수있다.

그리고 str_to_date()라는 함수가 있는데 date_format()과 비슷하며 아래와 같이 사용할수 있다.

![](https://velog.velcdn.com/images/lionjy06/post/5801474c-7d7f-451b-a437-1edd3a04a329/image.png)



## Conditional statement

기타 이지만 내가 생각했을때 진짜 중요한 함수들

IF => sql에서도 조건문을 사용할수 있는지 처음알았을때는 너무 신기하고 유용했다.

![](https://velog.velcdn.com/images/lionjy06/post/270b9977-7797-4e78-8f5e-33dc50c6e95a/image.png)

CASE => 이것도 진짜 무조건 알아야한다... 현재 맡고 있는 프로젝트에서 골머리를 앓고 있던 로직을 이것으로 해결한경험이 있는데 진짜 너무 신기하면서 좋았다.

CASE는 if, else if, else와 같은 역할을 한다고 보면된다.

사용법

CASE
WHEN 조건문1 THEN 실행문
WHEN 조건문2 THEN 실행문
ELSE 실행문
END

![](https://velog.velcdn.com/images/lionjy06/post/7534e174-a61e-411d-82e2-4d92ebbb2ffb/image.png)

IFNULL(a,b) => a가 null이면 b 가 출력이긴한데 이거는 한번도 안써봤다...