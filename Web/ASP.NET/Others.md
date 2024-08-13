# Others

## 파일 처리
정적 파일을 다룰 때에는 wwwroot를 사용하는 것이 좋으며, IHostingEnviroment를 생성자 주입으로 받아 WebRootPath를 통해 경로를 얻을 수 있다. 이후 Path.Combine등으로 상세 경로를 설정한다.
### 파일 업로드
input type=file은 매개변수로 ICollection<IFormFile>로 받는다. IFormFile의 CopyToAsync를 사용해 파일을 저장한다.

### 파일 다운로드
액션 메소드의 반환 타입을 FileResult로 지정하면 다운로드가 가능하다. return File(바이트배열, "컨텐츠 타입(application/octet-stream)", 파일이름)을 사용하면 된다.

## Https
### 개발 단계
cmd에서 dotnet dev-certs https --trust를 실행한다.
### 배포 단계
호스팅 서비스나 SSL 인증 기관을 통해 SSL 인증서를 구입하고, 서버에 설치