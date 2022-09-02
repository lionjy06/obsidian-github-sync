

docker run은 다음에 나올 이미지를 실행시키는 명령어지만 이미지명 다음 나오는 명령어는 기존에 이미지가 가지고 있는 명령어를 무시하고 이미지명 다음에 나오는 명령어를 실행시킨다.

근데 무시하고 실행시키는 명령어 자체가 이미지에 없으면 아예 실행이 안됌. => docker run hello-world 는 run이라는 시작 명령어를 이미 이미지에 내장하고 있지만 만약 docker run hello-world ls를 친다면 에러를 리턴함 왜냐면 hello-world는 run은 있지만 ls를 명령어로서 가지고있지 않기때문이다.

docker ps => 실행중인 도커 컨테이너의 상태를 볼수가있다.
docker ps -a => 실행중이거나 실행중이지 않은 모든 컨테이너의 상태를 볼수있다.

docker ps --format(원하는 정보만 보고싶을때) 'table{{.Names}}\table{{.Image}}' => 컨테이너이름이랑 이미지만 보고 싶을때 이렇게 하면됨



### 컨테이너의 생명주기
생성 -> 시작 -> 실행 -> 중지 -> 삭제

docker run <이미지명> <명령어> : 도커 컨테이너를 생성, 시작 실행까지 한번에 다하는 명령어가 run
docker create <이미지명> : 도커 컨테이너 생성
docker start <컨테이너명>: 컨테이너 시작

docker run을 둘로 쪼갠것이 create 와 start이다


<중지>
docker stop vs docker kill
-> 둘다 실행중인 컨테이너를 멈추다

docker stop(kill) <컨테이너명>

docker stop: gracefully stop

docker kill: stop immediately

<삭제>
docker rm <컨테이너명> : 컨테이너 삭제
docker stop <컨테이너명>: 컨테이너 중지

--> 컨테이너를 삭제하고 싶다면 먼저 컨테이너를 중지하고 삭제해야한다.

docker rm `docker ps -aq` => 모든 컨테이너를 삭제한다

docker rmi <이미지명>: 이미지 삭제

docker rmi `docker images`: 모든 이미지 삭제

docker system prune: 실행중인 컨테이너 말고는 전부다 없에버려서 사용안하는건 싹 정리할수있다.


<실행중인 컨테이너에 명령어 전달>
docker를 run시켜 놓고 실행중인 컨테이너에 명령어를 전달하는 키워드는 exec이다.

docker exec <컨테이너> <명령어> => 실행중인 컨테이너에 명령어를 전달한다.

docker run <컨테이너> + docker exec <컨테이너> <명령어> = docker run <컨테이너명> <명령어> 

-> 실행이 되어있지 않다면 상관없지만 이미 실행되어 있는 컨테이너에 명령어를 전달하는건 docker run <컨테이너> <명령어> 보다 docker exec <컨테이너> <명령어>가 적절하다.


레디스 도커에서 실행시키기
1. docker run redis 를 이용하여 redis 이미지를 pull 받음과 동시에 start를 한다. (Redis server on)
2. 새로운 터미널 환경에서 Redis server로 통신하기 위해 명령어를 입력한다.
3. 하지만 connection refused... 왜 그럴까 => 왜냐하면 redis server는 현재 도커 컨테이너 안에서 구동중인데 새로운 터미널 즉 redis client는 컨테이너 밖에 있기 때문에 연결이 안됀다. 그럼 어떻게 해야할까?
4. docker exec를 통해 client도 실행중인 서버에서 명령어를 전달할수있게 해야한다.
5. docker exec <레디스 서버 컨테이너 ID> redis-cli
6. 근데 아무일도 안일어난다;; 이건 또 왜?? 왜냐하면 -it(i: interactive t:terminal it는 즉 터미널과 상호 작용하겠다는 뜻이다.)옵션을 안주었기 때문. -it는 실행중인 컨테이너와 연결하고 계속해서 그 환경에서 작업을 할수있게 도와준다. -it없이는 실행시키고 바로 나와버려서 추가적인 명령어 입력이 불가능하다.
7. docker exec -it <redis container ID> redis-cli 

* docker run을 할때 -d 옵션을 주면 백그라운드(데몬) 에서만 돌아가기 때문에 새로운 터미널을 열 필요가 없다.
docker run -d redis 

docker exec -it <컨테이너> <자신이 사용하는 shell>
ex. docker exec -it <컨테이너명> sh(bash)
