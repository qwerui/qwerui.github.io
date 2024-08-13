# Baked Lighting

## Light
Unity의 Light는 3가지 모드를 제공한다.   
Realtime : 매 프레임 마다 연산을 수행해 빛과 그림자를 표현한다. 연산량이 많다는 단점이 있다.   
Baked : 미리 빛과 그림자를 연산해 저장해둔다. 런타임 오버헤드가 없다.   
Mixed : 정적인 객체에는 Baked, 동적인 객체에는 Realtime을 사용한다.

## Light Map
라이트 맵은 주로 정적인 객체를 대상으로 밝기, 그림자 등을 미리 저장하는 텍스쳐다.   
기본이 되는 오브젝트에 라이트맵을 블렌딩한 결과가 출력되기 때문에 동적인 오브젝트가 그림자 위에 있더라도 어두워지지 않는다.   
   
**사용 순서**   
1. Lighting Setting을 생성한 뒤 파라미터를 설정한다.
2. Scene의 Light들의 모드를 Baked나 Mixed로 변경한다.
3. 라이트맵의 영향을 받을 오브젝트들의 Static에서 Contribute GI를 체크한다.
4. 굽는다.

Lighting Setting의 각 파라미터의 설명은 공식문서를 보자 : [Light Setting](https://docs.unity3d.com/kr/2023.2/Manual/class-LightingSettings.html)   

Mixed Lighting의 각 모드는 다음과 같은 결과를 낸다.   
Baked Indirect : 간접광의 정보만 저장한다. 직접광과 그림자는 Realtime으로 연산한다. 3모드 중 가장 비용이 비쌈.   
Subtractive : 직접광, 간접광, 그림자를 저장한다. 가장 비용이 적지만, 동적인 객체와의 그림자에 경계가 생기는 단점이 있다.   
Shadow Mask : 별도의 그림자 텍스쳐를 생성한다. 추가적인 메모리가 필요한 대신 동적인 객체와 그림자가 자연스럽게 연결된다. Baked Indirect와 Subtractive의 중간적인 성질. Project Settings의 Quality에서 거리에 따른 퀄리티 차이를 줄지 설정할 수 있다. 가까울 수록 디테일해짐. 
   
### Light Map을 사용할 때 고려할 점
텍스쳐를 저장해 놓았기 때문에 단순히 생각해도 메모리가 추가로 필요하다.   
드로우콜 배칭에 영향을 미친다.   
Directionality 사용 유무 => Normal맵을 고려하는 옵션이다. 추가 메모리가 발생할 수 있다.   
Shadowmask 사용 유무 => 그림자 정보를 따로 저장한다. 추가 메모리가 발생할 수 있다.

## Light Probe
라이트 프로브는 주로 동적인 객체또는 크기가 작은 정적인 객체를 타깃으로 한다.   
라이트 프로브가 위치한 정점을 기준으로 빛을 보간하며, diffuse term만 가능하다(specular term 불가).   
30개의 부동소수점(각 컬러채널 9개 * 3 + 위치정보 3개) + 그외 정보로 저장하기 때문에, 데이터의 크기가 작다.   

**사용 순서**
1. Light Probe Group을 적절하게 배치한다. ※ 정점이 안보일 경우 레이아웃을 기본 레이아웃으로 변경해보자.
2. 광원의 모드를 Mixed로 설정한다. (Baked모드 광원은 작동하지 않는다.)
3. 광원의 영향을 받는 객체의 Probes를 Blend Probes로 설정한다. 정적인 객체는 Receive GI를 Light Probes로 설정한다.

## Reflection Probe
Specular term을 위한 데이터, Cubemap Texture(Texture * 6)로 저장하기 때문에 메모리 이슈가 발생할 수 있다.   
단순히 Reflection Probe를 Scene에 두고 반사체를 두면 반사체에 주변 객체가 보이게된다.