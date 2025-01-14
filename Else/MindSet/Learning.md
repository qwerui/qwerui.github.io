# Learning

## 학습을 위한 방안
1. 상황 파악이 우선이다. 회의나 운영 이슈, 코드 리뷰 등에 참여하는 게 도움된다.
2. 코드가 어떻게 동작하는지 알아두자. 예외, 스택 트레이스 출력, 디버거를 통해 파악할 수 있다.
3. 문서를 읽는 습관을 가지자.
4. 제한 시간을 정해두고 스스로 해결해보자, 시간을 넘기면 도움을 청하고, 제한 시간을 늘리는 경우는 진척이 있을 경우만이다.
5. 질문을 할 때에는 자신이 파악한 내용을 전달해야한다.

## 방어적 프로그래밍
1. null을 지양하자. 널 객체 패턴이나 옵션 타입을 사용하는 편이 좋다.
2. 불변 변수를 활용하자.
3. 타입 힌트와 정적 타입 검사를 사용하자.
4. 입력값을 검사하자.
5. 예외를 활용하자. 다만, 구체적으로 사용해야한다.

## 로그 등급
TRACE : 줄 단위 로그나 데이터 구조 확인
DEBUG : 프로덕션 상황에서 문제가 발생한 상황
INFO : 상태에 대한 로그
WARN : 잠재적인 오류가 발생할 가능성이 있는 상황
ERROR : 살펴봐야할 에러가 발생한 상황
FATAL : 프로그램을 즉시 중단시켜야할 만큼 치명적인 상황

## 버전
MAJOR : 하위 호환성을 갖지 못하는 변경을 추가할 때 증가
MINOR : 하위 호환성을 갖는 기능을 추가할 때 증가
PATCH : 하위 호환성을 갖는 버그 수정할 때 증가

## 코드 리뷰
### 리뷰이
리뷰를 요청할 때 리뷰어, 제목, 설명을 포함해야하며, 설명은 테스트 방법, 다른 자료에 대한 링크, 구현 상세에 대한 도움 요청, 질문, 체크리스트 등이 들어가야한다.

### 리뷰어
1. 리뷰를 긴급도에 따라 선별하고, 리뷰를 위한 시간을 할당한다. 리뷰 때문에 하던 일이 중단되어서는 안된다.
2. 코드의 변경사항을 이해하고, 포괄적인 피드백을 제시한다.
3. 좋은 점은 인정하자. 코드를 통해 새로 배운 것이 있다면 리뷰이에게 언급하는 것도 좋다.
4. 중요한 이슈에 먼저 신경써야한다. 코드 스타일 같은 사소한 것을 찾는데 집중해선 안된다.
5. 테스트 코드도 리뷰해야한다.
6. 완벽하지 않아도 좋다. 완벽을 추구하다가 시간이 걸리게 되면 생산성이 떨어진다.

## 설계 문서
### 템플릿
1. 개요
2. 현재 상태와 컨텍스트
3. 변경 이유
4. 요구사항
5. 전체 해결책
6. 채택한 해결책
7. 설계와 아키텍처 (시스템 다이어그램, UI/UX 변경, 코드 변경, API 변경, 영속 계층 변경)
8. 테스트 계획
9. 롤아웃 계획
10. 미결 사항
11. 부록

## 애자일
1. 절차와 도구보다는 상호작용
2. 문서보다는 소프트웨어
3. 계약 협상보다는 고객과의 협업
4. 계획보다는 임기응변