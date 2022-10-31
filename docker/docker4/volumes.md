volume

volume은 도커 컨테이너와 로컬 소스코드를 매핑시켜 소스코드의 변경이 있을시 도커가 이미지 재빌드없이 바로 소스코드를 적용시키는 것이다.

-   종속성에 변경이 있을시에는 재빌드해줘야하는것이 맞음.
    

docker run -p <외부포트>:<내부포트> -v <workdir경로>/node_modules -v $(pwd) :<workdir> <이미지명>

:를 기준으로 왼쪽(pwd)는 로컬(참조하는 소스가 있는 로컬) <workdir 경로>는 도커. 두개를 매핑시켜 서로 바라보게 할수있다.

node_modules는 앞에 참조하는 부분이 없기때문에 아무것도 참조하지않는다. 즉 node_modules는 가져오지않는다.

docker run -d -p 8080:8080 -v /practice_backend/node_modules -v $(pwd):/practice_backend lionjy06/nodejs