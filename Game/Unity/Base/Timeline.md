# Timeline

## 개요
Animation과 비슷하지만, 더 범용적인 기능이다. 애니메이션 재생, 오브젝트 활성화/비활성화, 파티클 재생, 카메라 이동 등 여러 기능을 시간에 따라 배치하고 블렌딩할 수 있다. 스킬 등의 컷인에 사용할 수 있다.

## 사용법
1. 씬에 Playable Director를 가진 오브젝트를 배치하고 Timeline 에셋을 연결한다.
2. Timeline Editor에서 트랙을 작성한다.

### 트랙 목록
- Activation Track : 오브젝트 활성화/비활성화 설정
- Animation Track : 애니메이션 제어
- Audio Track : 소리 제어
- Control Track : 프리팹을 인스턴스화 하거나 다른 타임라인을 포함할 경우 사용
- Playable Track : 스크립트 실행
- Signal Track : 이벤트 기반 스크립트 실행
- Cinemachine Track : Cinemachine 제어, 카메라 이동이나 블렌딩이 가능하다.