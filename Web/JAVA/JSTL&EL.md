# JSTL & EL

## 설치

1. maven repository에서 JSTL을 검색해 servlet에 해당하는 JSTL을 다운받아 lib에 넣는다
2. jakarta.servlet.jsp.jstl과 jakarta.servlet.jsp.jstl-api를 다운 받아 lib에 넣는다
3. JSP 파일에 <%@ **taglib** uri=*"http://java.sun.com/jsp/jstl/core"* prefix=*"c"* %>를 선언한 뒤 c:태그로 사용한다. (코어의 경우, 모듈이 다른 경우도 존재)

## 사용

```jsx
//서블릿에서 포워딩
request.setAttribute("last", last);
request.setAttribute("fiboBuffer", fiboBuffer);
request.getRequestDispatcher("/example2.jsp").forward(request, response);

//jsp에서 사용
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Scripting Elements Example</title>
</head>
<body>
	<c:set var="count" value="0"/>
	// Attribute로 넘긴 값을 자동으로 인식한다. EL을 통해 확인 가능
	<p>50번째 피보나치 숫자 : ${last}</p>
	<hr>
	<c:forEach var="item" items="${fiboBuffer}">
		<p>${count}번째 피보나치 숫자=${item}</p>
		<c:set var="count" value="${count+1}"/>
	</c:forEach>
   
</body>
</html>
```

## JSTL 태그

- Core : 변수, if, for 등 기본 프로그래밍 코드의 기능에 페이징 이동 기능을 제공
- Formatting : 문자열이나 컬렉션을 형식화, 다국어 지원 기능을 제공
- Sql : 데이터베이스와 상호작용하기 위한 기능을 제공
- Functions : 문자열을 처리하는 함수를 제공

### Core

```jsx
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<c:out value="출력 내용"/> // 출력에 사용
<c:set var="변수이름" value="값"/> // 변수 선언
<c:remove var="변수이름"/> // 변수 삭제
<c:catch var="변수이름"> //예외처리, 변수에 예외의 내용이 들어간다.
<c:if test="조건문"> //단일 조건문 처리 else는 없다
<c:choose> // 다중 조건문 처리
	<c:when test="조건문"> // 조건문이 참 일때 수행
	</c:when>
	<c:otherwise> // else와 같은 역할
	</c:otherwise>
</c:choose>
<c:import url="페이지경로" var="변수이름"> //jsp:include와 유사, 변수에 페이지의 내용을 저장할 수 있음
	<c:param name="파라미터 이름" value="값"/> //다른페이지로 넘길 파라미터를 지정, url이나 redirect에도 사용가능
</c:import>
<c:forEach var="변수이름" begin="시작값" end="끝값"> //시작값에서 끝값까지 반복
</c:forEach>
<c:forEach var="변수이름" items="컬렉션or배열"> //컬렉션이나 배열의 요소 반복
</c:forEach>
<c:forTokens var="변수이름" items="구분자를 포함한 문자열" delims="구분자"> //문자열을 구분자로 나눠 순회   
</c:forTokens>
<c:url var="변수이름" value="url경로" var="변수명" scope="영역"/> //URL을 재작성하는데 사용
<c:redirect url="url경로" context="컨텍스트 경로"/> //설정한 경로로 이동
```

### Functions

```jsx
contains() //문자열 포함 여부
containsIgnoreCase() //대소문자 무시하고 문자열 포함 여부
startsWith() //문자열로 시작 여부
endsWith() //문자열로 종료 여부
escapeXml() //문자열에 포함된 특수문자를 특정 코드로 변환
indexOf() //검색 대상 문자열 위치
split() //문자열을 구분자로 분리
join() //배열 형태의 문자열을 구분자로 연결
length() //문자열 길이
substring() //특정 위치 문자열
substringAfter() //설정한 문자열 이후의 부분 문자열
substringBefore() //설정한 문자열 이전의 부분 문자열
replace() //검색 대상 문자열을 치환
toLowerCase() //모두 소문자 변환
toUpperCase() //모두 대문자 변환
trim() //문자열 앞뒤 공백 제거
```