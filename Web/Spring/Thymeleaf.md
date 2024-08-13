# Thymeleaf

## 표현식

- 변수 표현식 : ${…}
- 선택 표현식 : *{…} (th:object에 들어있는 값을 가져올 때 사용)
- 메시지 표현식 : #{…}
- 링크 URL 표현식 : @{…} (||안에서는 ${…}을 사용 가능하다)

### 기본값 지정

표현식에 ?:를 붙이면 기본값을 지정할 수 있다. 이는 표현식 ? 입력값 : 기본값과 같다.

### 인라인

[[표현식]]으로 태그 외부에서 타임리프의 표현식을 사용할 수 있다. script태그에 th:inline=”javascript”속성을 지정하면 자바스크립트에서도 사용할 수 있다.

## 표현식 기본 객체

- #ctx : context object
- #vars : context variables
- #locale : context locale
- #httpServletRequest : HttpServletRequest
- #httpSession : HttpSession

## 표현식 유틸리티 객체

- #dates : java.util.Date
- #calendars : java.util.Calendar
- #numbers : 숫자 포맷팅
- #strings : String에 대한 기능, contains, startsWith 등
- #objects : 일반적인 객체들에 대한 기능
- #bools : boolean 평가에 대한 기능
- #arrays : 배열
- #lists : 리스트
- #sets : 셋
- #maps : 맵
- #aggregates : 배열이나 리스트의 집계
- #messages : 변수 표현식에서 외부 메시지를 얻는 기능
- #ids : 반복되는 id속성을 다루는 기능, 예를 들면 결과 순회

## 속성

th:[HTML 속성]으로 HTML 속성에 타임리프 문법으로 값을 넣을 수 있다.

## 반복

th:each=”item, status : ${array}”로 태그를 반복할 수 있다. item자리에 개체, status자리에 반복 상태값들이 들어있다.

## 조건

th:if로 조건식에 따라 태그를 활성화/비활성화 할 수 있고, th:switch를 통해 하위 태그의 th:case에 맞는 태그만 활성화 할 수 있다.

## 템플릿

태그에 th:fragment로 템플릿의 이름을 지정하고 th:include나 th:replace로 해당 위치에 템플릿을 삽입할 수 있다. 삽입할때에는 파일명::템플릿이름 형식으로 지정해야한다. include는 태그 내부 내용만 포함시키고, replace는 태그 그 자체를 대체한다.

## 지역변수

th:with=”변수명=값”으로 지역변수를 만들 수 있다.

## 주석

<!—/* 타임리프 파싱 할 때 제거됨 */—>

## th:block

실제로 렌더링되지 않지만, 그룹화를 위해 존재하는 논리적인 태그

<hr/>

튜토리얼 링크 : [Tutorial: Using Thymeleaf](https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html)