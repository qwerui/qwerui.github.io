# Assembly Definition

## Assembly Definition 개요
Assembly를 간단하게 정의하면 코드의 묶음이다. Assembly Definition은 이 코드들을 하나의 Assembly로 묶어주는 텍스트 파일이다. 사용법은 단순히 하나의 Assembly로 묶고 싶은 폴더에 Assembly Definition을 만들면 끝이다. 이후는 Unity가 알아서 생성된 위치에서 부터 하위 스크립트들을 생성한 Assembly에 넣어준다.   
![Assembly](/images/Assembly.PNG)   

## 이점
1. 컴파일 타임 감소
프로젝트를 처음 만들었을 경우에도 Assembly는 존재한다. 그것은 Assembly-CSharp이다. 우리가 스크립트를 생성했을 경우, 이 Assembly에 추가되는 것을 볼 수 있다. Unity는 변경된 Assembly만 재컴파일하는데, Assembly를 나누지 않았다면 문자 1개 추가했다고 모든 스크립트를 재컴파일해야한다.   
다만 이 효과를 보기 위해서는 가능하면 모든 코드에대해 어셈블리를 분리하는 것이 좋다. Assembly Definition을 통해 생성한 Assembly는 자동으로 Assembly-CSharp에 의존한다. 따라서 Assembly-CSharp에 속하는 스크립트를 수정하면 모든 스크립트를 재컴파일해야한다.

2. 의존성 관리
Assembly는 기본적으로 다른 Assembly의 코드를 모른다. Assembly는 dll의 형태로 존재하기 때문이다. 이점을 이용해서 필요한 코드에만 의존하도록 관리할 수 있다.

## 의존성 추가
![Assembly_Reference](/images/AssemblyReference.PNG)   
Assembly Definition 에셋에서 Reference에 참조할 Assembly를 추가하면된다.