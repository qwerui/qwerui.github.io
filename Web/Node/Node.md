# Node

## Node 개요
자바스크립트 언어를 실행시키는 환경, 크롬 V8 엔진을 사용한다. npm을 사용해 외부 모듈을 자유롭게 사용가능하고, 다양한 플랫폼과 데이터베이스를 지원한다. 기본적으로 싱글 스레드로 동작하지만 인스턴스 확장을 통해 멀티 스레드 수준의 성능을 낼 수 있다.   
이벤트 루프를 활용해 오래 걸리는 I/O 작업을 하는 동안 다른 처리를 할 수 있다.(Non-Blocking)
<br/>

**장점** : I/O 처리를 잘하는 노드를 서버로 사용한다. (네트워크, 데이터베이스, 디스크 작업 등)   
**단점** : CPU부하가 큰 작업에는 부적합. (이미지, 비디오 처리 등)   
=> 개수는 많지만 크기는 작은 데이터를 실시간으로 주고 받는 작업에 적합, 짧은 시간에 대량의 클라이언트 요청에 대응하는 웹 어플리케이션에 적합

## NPM
Node Package Manager, Node.js의 패키지/모듈을 관리한다. 다운받은 패키지는 package.json에서 버전이나 의존성 등을 관리한다. package.json을 미리 준비해두면 npm install만으로 package.json에 있는 모듈들을 한 번에 설치할 수 있다.   

**npm 명령어**   
- npm init : 패키지 정보 설정
- npm install 패키지명 : 패키지 설치
- npm uninstall 패키지명 : 패키지 삭제
- npm update 패키지명 : 패키지 업데이트
- npm search 키워드 : 키워드가 포함된 패키지명 검색
- npm start/run : 어플리케이션 실행
<br/>

**package.json**   
- name : 프로젝트 이름
- version : 프로젝트 버전
- description : 프로젝트 설명
- main : 해당 패키지 진입점 모듈
- script : 복잡한 명령을 npm 이용해 단순화
- author : 제작자 정보
- license : 라이선스
- keywords : npm에서 패키지 찾을 때 사용
- bugs : 버그 발생 시 연락처
- dependencies : 일반적인 배포환경에서 필요한 의존성 모듈
- devDependencies : 개발환경에서 필요한 의존성 모듈

## Node.js의 내장 모듈
- os : 운영체제 정보
- path : 파일 경로
- url : 인터넷 주소
- querystring : 기존 노드의 url 사용할 때, search 부분을 사용하기 쉽게 객체로 만드는 모듈
- crypto : 암호화
- util : 편의 기능
- fs : 파일 시스템
- http : 웹서버 모듈

## Node.js 주요 미들웨어
static : 특정 폴더의 파일을 특정 패스로 접근
morgan : 클라이언트의 요청 로그 확인
body-parser : body에 포함한 데이터를 JSON 형태로 파싱
cookie-parser : 헤더에 포함한 쿠키를 JSON 형태로 파싱
multer : 파일 업로드
express.static : 정적 파일 제공
passport : 사용자 인증