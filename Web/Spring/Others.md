# Others

## 이미지

servlet-context.xml에 <resources mapping=”url패턴” location=”폴더 경로”>를 기재, 되도록 resources 폴더만 기재하고 사용할 때 resources 기준으로 경로를 사용하는 것을 권장
```html
<img src="${pageContext.request.contextPath}/resources/folder/image.png">
```

## 절대경로와 상대경로

- 절대경로는 파일의 위치를 추적하기 쉽지만 컨텍스트 경로가 변경되면 에러가 발생한다.
- 상대경로는 컨텍스트 경로가 변경되어도 작동하지만 파일의 위치를 추적하기 어렵다.
- JSTL의 c:url을 이용하면 컨텍스트 경로가 변경되어도 실행되게 할 수 있고, 파일의 위치를 추적하기에도 용이하다.

## 리다이렉팅

뷰(jsp)를 찾는 문자열에 redirect:를 맨 처음에 붙이게 되면 리다이렉팅이 된다. 리다이렉팅은 request나 response를 갖지 못하지만, 억지로 데이터를 넘기기 위해서는 RedirectAttributes 클래스를 통해 데이터를 넘길 수 있다. 이 방법은 세션에 데이터가 저장되며 일반적으로 권장하지 않는 방법이다.

## 뷰 컨트롤러

xml에서 직접 URL과 뷰를 컨트롤러 없이 직접 매핑하기 위해 사용, mvc 네임스페이스가 활성화 되어있어야하며, 일반적으로 사용하지 않음
```html
<view-controller view-name="home" path="/"/>
```

## ModelAndView
```java
@Override
	protected ModelAndView handleRequestInternal(HttpServletRequest request, HttpServletResponse response)
			throws Exception {
		ModelAndView mav = new ModelAndView();
		mav.setViewName("hello");
		mav.addObject("msg", "Classic MVC");
		mav.addObject("msg2", "Hello MVC");
		return mav;
	}
```

## 파일 업로드

파일 업로드를 위해서는 commons-fileupload 의존성 설정을 추가해야한다. 그 후 servlet-context.xml에 MultipartResolver 빈 설정을 추가한다.

```jsx
//속성 목록
//defulatEncoding : 파싱할 때 사용할 인코딩 지정
//maxUploadSize : 최대 업로드 크기
//maxInMemorySize : 임시 파일을 생성하기 전에 메모리에 보관할 수 있는 최대 바이트
//preserveFilename : 파일 이름에서 경로 정보를 제거하지 않고 클라이언트가 보낸 파일 이름을 유지할 지 여부
<beans:bean id="multipartResolver" 
class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
	<beans:property name="maxUploadSize" value="50000000"/>
</beans:bean>
```

그 후 form 태그에 enctype=”multipart/form-data” 속성을 추가한다.

## 파일 다운로드

파일을 다운로드하는 핸들러 메서드의 리턴타입은 ResponseEntity<T>다. 

- 다만 최근에는 서블릿3.0에서 제공하는 업로드/다운로드를 사용하는 경우가 많다.

### 썸네일 이미지 생성
```java
int width = 100;
int height = 100;

ByteArrayInputStream imageArray = new ByteArrayInputStream(fileData);
BufferedImage srcImage = ImageIO.read(imageArray);//라이브러리 설치 필요

Image resizeImage = srcImage.getScaledInstance(width, height, Image.SCALE_FAST);

BufferedImage outputImage = new BufferImage(width, height, srcImage.getType());

Graphics g = outputImage.getGraphics();
g.drawImage(resizeImage, 0, 0, null);
g.dispose();

File outputFile = new File(uploadDir, "T-"+uuidFileName);
ImageIO.write(outputImage, fileExtension.substring(1), outputFile);
```

## 배포 관련
application.properties의 내용을 docker run -e로 덮어쓸수 있다. application.properties에 기재한 내용을 대문자로 바꾸고, .을 _로 바꾼다.