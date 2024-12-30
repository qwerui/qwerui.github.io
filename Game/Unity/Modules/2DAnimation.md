# 2D Animation

![출처](https://www.youtube.com/watch?v=b3J2SInvuwM)

## 본 리깅 애니메이션
![Animation_Rig](/images/Animation_Rig.PNG)   
위 사진은 Animation 2D 패키지의 샘플이다. PSD Importer 등으로 Import한 텍스쳐의 Sprite Editor에서 Skinning Editor로 가면 위와 같은 화면이 된다. 각 메뉴의 역할은 아래와 같다.
- Pose : 포즈를 확인하거나 되돌릴 수 있다
- Bones : 본을 추가하거나 수정할 수 있다. Create Bone에서는 좌클릭이 추가, 우클릭이 완료다.
- Geometry : 본에 영향을 받는 버텍스를 조정한다.
- Weights : 본과 버텍스를 연결한다. 버텍스가 어느 본의 영향을 받을지 가중치를 결정한다.
- Rig : 완성된 리깅을 복사하거나 붙여넣는다.
<br/>

대체로 다음과 같은 과정을 거치면 된다.
1. Auto Geometry로 메시를 결정한다.
2. Create Bone으로 Bone을 추가한다.
3. Auto Weights로 Bone과 메시를 연결한 뒤, Weight Brush로 가중치를 수정한다.
4. 애니메이션을 적용할 프리팹에 Animation Skin을 추가한 뒤 Create Bone을 누른다.
5. 애니메이션 클립을 생성하고 레코드를 통해 본을 움직이면서 애니메이션을 생성한다.

## Sprite Library & Sprite Resolver
- Sprite Library : 스프라이트의 집합, 스프라이트마다 카테고리를 나누고 라벨을 붙일 수 있다. 카테고리와 라벨만 같다면 같은 애니메이션을 다른 스프라이트 라이브러리 에셋이 공유할 수 있다.
- Sprite Resolver : Sprite Library에서 카테고리와 라벨을 기준으로 스프라이트를 찾아 렌더러에 적용시킨다. 같은 카테고리는 하나의 Sprite Resolver에서 하나로 묶인다. Animation Clip에서 라벨을 바꾸는 것으로 스프라이트를 변경할 수 있다.

### Aseprite
도트 그래픽 툴, Unity에서 Import를 지원한다.