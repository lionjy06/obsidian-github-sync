## 도커를 왜 쓸까? 
 어떠한 프로그램을 다운받는 과정을 간단하게 만듬 -> w/o 도커 인스톨러를 따로 다운받아서 실행하여 설치를 완료함 but 가끔 fail한다. 왜냐면 서버, 패키지 버전, 운영체제 등 다양한 이유로 에러가 뜬다. 

도커 없이도 프로그램을 설치할수있으나 도커는 간편함을 제공한다... (docker run -it redis만 쳐도 다운받아짐)


## 도커란 무엇인가
도커란 무엇인가 에 대해 한마디로 정의하기는 어렵다... 

컨테이너란? 서버에서의 컨테이너란 다양한 프로그램 및 실행환경을 컨테이너로 추상화 하여 동일한 인터페이스를 제공하여 배포 및 관리를 단순하게 만든다. -> 이것저것 프로그램에 필요한 언어, db 등을 한곳에 담에 관리하기 쉽게 만든다. 

=> 코드와 모든 종속성을 패키지화 하여 한 컴퓨팅 환경(ex 로컬)에서 다른 컴퓨팅 환경(ex. aws, azure, gcp와 같은 배포 환경)으로 빠르고 안정적으로 옮겨 실행하도록 하는 소프트웨어 단위. = 간단하고 편리하게 프로그램을 실행시켜주는 것

## 도커 이미지와 컨테이너의 정의

도커 이미지를 기반으로 도커 컨테이너를 실행시킨다.

도커 이미지: 실행에 필요한 코드, 런타임, 시스템 도구, 라이브러리에 대한 정보를 담고 있는 소프트웨어 패키지

도커 컨테이너: 도커 이미지를 기반으로 빌드하여 실행시켜 돌아가는 소프트웨어 프로그램 단위

* 이미지를 빌드시켜 컨테이너를 만듬

