# Refactoring

## 품질 속성
- 이해 가능성 : 설계를 어느 정도 이해할 수 있는가?
- 변경 가능성 : 기존 기능 변경 시, 사이드 이펙트 없이 쉽게 수정이 가능한가?
- 확장 가능성 : 기능 추가 시, 사이드 이펙트 없이 쉽게 추가가 가능한가?
- 재사용 가능성 : 다른 문제에서 사용이 가능한가?
- 테스트 가능성 : 테스트를 통해 결함을 쉽게 감지할 수 있는가?
- 안정성 : 실행 과정에서 문제점 발생 시 효과적으로 방어할 수 있는가?

## 고려해볼만한 것
1. 매직 넘버 -> 상수
2. assert 도입
3. 분류 코드 -> 클래스
4. 에러 코드 -> exception
5. 생성자 -> 팩토리 패턴
6. 상속 -> 위임(ex: 컴포넌트 패턴)
7. delegate 은폐
8. null 객체

## 복잡도와 협력자
- 간단한 코드 : 복잡도 낮음, 협력자 적음
- 컨트롤러 : 복잡도 낮음, 협력자 많음
- 도메인 : 복잡도 높음, 협력자 적음
- 복잡한 코드 : 복잡도 높음, 협력자 많음 => 컨트롤러와 도메인으로 나누는 대상