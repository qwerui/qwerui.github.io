# Cinemachine

## 개요
씬에 Cinemachine의 Virtual Camera를 생성하면 CinemachineVirtualCamera 컴포넌트를 가진 오브젝트가 생성된다. 그와 동시에 카메라에는 Cinemachine Brain 컴포넌트가 생성된다.   

![Virtual Camera](https://docs.unity3d.com/kr/Packages/com.unity.cinemachine@2.3/manual/CinemachineVirtualCamera.html)

## 카메라 종류
- FreeLook Camera : Virtual Camera + Axis Control(이동) + Orbits(회전)
- Blend List Camera : 여러 Virtual Camera를 전환할 수 있음
- State Driven Camera : 지정한 오브젝트의 애니메이션 상태에 따라 Virtual Camera를 전환할 수 있음
- Clear Shot Camera : 차폐물 등에 의해 가려지는 현상을 보정
- Dolly Camera With Track : Follow를 따라 지정된 경로를 통해 이동한다.
- Dolly Track With Cart : 경로와 경로를 따라가는 카트를 생성한다. 카메라를 포함한 오브젝트를 카트의 하위 오브젝트로 넣어주면 카트를 따라서 이동한다.
- Target Group Camera : 여러 대상을 카메라 안에 비출 수 있도록 한다.
- Mixing Camera : 2개의 Virtual Camera를 생성해 가중치에 따라 전환할 수 있다.
- 2D Camera : 2D용 Virtual Camera