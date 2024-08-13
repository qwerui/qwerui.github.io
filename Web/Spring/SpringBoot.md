# Spring Boot

## 어노테이션
- @Mapper : Mybatis의 DAO가되는 인터페이스

## Thymeleaf

html 태그에 th:계열 속성을 통해 여러 기능을 부여한다.

@{/}는 contextpath를 의미, /가 없으면 현재 위치에서 상대경로 요청. @{}를 사용할 때 ||안에 기재하면 ${}사용 가능   
#{}은 Message Expression을 의미.   

#ctx는 표현식 기본 개체(Expression Basic Objects의 context object)를 의미.   
#calendars는 표현식 유틸리티 개체(Expression Utility Objects).   
#calendars.format(dto.regdate, 'yyyy-MM-dd')는 java.util.Calendar를 이용한 서식 출력