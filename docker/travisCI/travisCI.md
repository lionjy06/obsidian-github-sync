# Travis CI 흐름도 (CI 툴)
1. 로컬 git에 있는 코드를 코드저장소인 github에 push한다.
2. 만약 코드가 master or main에 push된다면 travisCI가 신호를 받는다.
3. master에 업데이트된 코드를 travisCI에 가져온다.
4. 테스트 코드를 돌려 본다
5. 성공한다면 aws나 gcp와 같은 호스팅 서비스에 넘긴다.