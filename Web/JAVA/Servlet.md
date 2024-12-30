# Servlet

## Web. xml

서블릿 설정 파일, 서블릿과 클래스, URL을 매핑한다. 어노테이션 방식으로도 사용 가능

```xml
<servlet>
    <servlet-name>ServletName</servlet-name>
    <servlet-class>com.package.class</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>ServletName</servlet-name>
    <url-pattern>/URL</url-pattern>
  </servlet-mapping>
  <!-- servlet-name을 기준으로 클래스와 URL이 매핑된다. -->
```

| 특징 | web.xml | 어노테이션 |
| --- | --- | --- |
| 도입 시기 | 서블릿 API 초기 버전 | 서블릿 3.0 이후 |
| 설정 위치 | 설정 파일 (web.xml) | 소스 코드 내 어노테이션 |
| 가독성 | 가독성이 떨어질 수 있음 | 설정과 코드가 함께 있어 가독성이 좋음 |
| 유연성 | XML 파일을 수정하여 설정 변경 가능 | 소스 코드 수정 후 재배포 필요 |
| 복잡성 | 복잡한 설정을 쉽게 관리 가능 | 간단한 설정에 적합 |