# Shadow Map

### Depth Map & Shadow Map
Depth Map : 카메라를 기준으로 픽셀까지의 거리를 저장한 텍스쳐   
Shadow Map : 광원을 기준으로 픽셀까지의 거리를 저장한 텍스쳐

### Shadow Distance
그림자를 처리할 거리, 작을 수록 부드러워지지만 그림자를 그릴 수 있는 거리가 짧아진다. 즉 먼 거리에 있는 그림자를 그릴 수 없다.

### Cascaded Shadow
![BeforeCascade](/images/BeforeCascade.PNG)
![AfterCascade](/images/AfterCascade.PNG)   
좌측 사진은 Cascade 적용 전이고 우측 사진이 적용 후다.   
좌측 그림자의 해상도가 낮은 것을 볼 수 있다.   
   
![ShadMapFrustumDiagram_FromUnity](/images/ShadMapFrustumDiagram_FromUnity.svg)   
잘못된 비유나 정보가 될지도 모르지만 내가 이해한 것을 서술한다.   
Shadow Map을 생성할 때 광원을 기준으로 픽셀까지 거리를 저장하는데, 이때 Directional Light면, 즉 빛의 진행방향이 모두 평행하다면, Light Direction을 법선벡터로 갖는 평면과 월드의 충돌판정으로 볼 수 있다.    
이 충돌판정은 해상도 단위로 이루어진다. 평면을 해상도만큼 쪼갠다는 것이다. 그럼 그림자를 나타내는 단위는 이 쪼개진 평면이 될 것이다.   
원근에 의해 우리 눈에 보이는(화면에 출력되는) 쪼개진 평면의 크기는 가까울 수록 클 것이고, 이는 우리 눈으로 봤을 때, 가까운 곳의 그림자에 있는 외곽의 계단모양이 크다는 것을 의미한다.   

![ShadMapCascadeDiagram_FromUnity](/images/ShadMapCascadeDiagram_FromUnity.svg)   
이를 해결하기 위해 절두체를 여러 영역으로 나눠 동일한 해상도를 가지면서 이 영역을 지나는 Shadow Map을 생성하면, 가까운 위치의 Shadow Map은 쪼개진 평면이 월드와 충돌하는 영역이 작아지기 때문에 더 정밀한 그림자를 생성할 수 있다.   
결론적으로 구분구적법과 비슷하다고 볼 수 있다. 큰 도형으로 공간을 채우는 것보다 작은 도형으로 공간을 채우는 것이 목표로한 공간에 더 정밀하게 채울 수 있기 때문이다.   
영역을 많이 나눌 수록 더 정밀해지지만, 더 많은 Shadow Map을 연산해야 하기에 오버헤드가 늘어난다.   
<br/>
[이미지 출처](https://docs.unity3d.com/kr/2019.4/Manual/shadow-cascades.html)