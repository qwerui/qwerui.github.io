# Lombok

## 개요
Java의 보일러플레이트(거의 또는 전혀 변경하지 않고 재사용할 수 있는 코드) 코드를 줄이기 위한 라이브러리, 어노테이션으로 원하는 코드를 생성해준다.

## 설치
1. Lombok 다운로드
2. cmd에서 lombok의 폴더에서 java -jar lombok.jar입력
3. IDE의 exe 지정
4. 설치

## 주요 어노테이션
- @Getter/@Setter
- @ToString
- @EqualsAndHashCode
- @NoArgsConstructor : 인수가 없는 생성자
- @AllArgsConstructor : 모든 필드를 인수로 갖는 생성자
- @RequiredArgsConstructor : final이나 @NonNull 필드를 인수로 갖는 생성자
- @Data : @Getter+@Setter+@ToString+@EqualsAndHashCode+@RequiredArgsConstructor
- @Builder : 빌더 패턴 생성
- @Value : 불변 클래스 생성, @Getter+모든 필드 private final+기본 생성자 private