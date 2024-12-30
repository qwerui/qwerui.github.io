# Normal Map
[출처](https://www.youtube.com/watch?v=fWVTePoMS5o)

## Normal Map 개요
요철로 인한 빛과 그림자를 표현하는데 사용되는 텍스쳐, 원래 오브젝트의 표면과 눈에 보이는 표면의 편차를 저장하며, 실제 폴리곤으로 요철을 표현하는 것보다 비용이 낮다. 빛의 방향의 반대 방향 벡터와 오브젝트의 표면(Normal)을 내적해 빛의 양을 계산한다.   

## Normal Map이 파란색인 이유
Normal Map은 원래 오브젝트의 표면과 눈에 보이는 표면과의 편차를 저장하기 때문에 벡터를 저장한다. 0,0,1을 기준으로 XY값의 차이를 더해 정규화한 값을 저장하는데, 벡터의 각 성분을 RGB로 대응시켜 텍스쳐에 저장한다. 이 때 XY의 차이가 없는 경우 0,0,1이 그대로 RGB값으로 변환되어 Normal Map이 파란색을 띈다.

## Normal Map의 처리
### Gamma Correction
Normal Map은 Gamma Space, Linear Space 상관 없이 적용되어야하기 때문에 특수한 처리과정을 거친다.   

사람의 눈이 어둠의 변화에 민감하게 반응하기 때문에 모니터는 원래 입력보다 어둡게 출력한다. 때문에 저장매체에 이미지를 저장 할 때는 원래보다 밝게 저장하는데 이 이미지를 저장하는 영역을 sRGB영역이라고 한다.   

Gamma 파이프라인은 Add같은 연산이 sRGB영역에서 이루어져 이미지가 밝은 상태에서 연산하고, Linear 파이프라인은 다시 어둡게 샘플링한 뒤 연산을 수행하기 때문에 Gamma에 비해 어두운 상태에서 연산한다.   

이 때문에 Linear에서 텍스쳐를 통한 이동 연산을 수행하면 의도했던 것보다 덜 가는 문제점이 생기는데 이 문제가 Normal Map에서도 발생한다. 따라서 Normal Map을 sRGB처럼 사용하지 않도록 처리해야한다.   

### Normal Map Compression
Normal Map도 텍스쳐기 때문에 메모리를 먹는다. 그래서 Normal Map을 압축하는 경우가 있는데, 일반 텍스쳐를 압축하는 경우보다 더 고퀄리티로 압축이 가능하다.   

Normal Map은 정규화된 벡터를 저장한다. 따라서 XYZ중 2개만 알아도 피타고라스 정리에 의해 남은 하나는 산출이 가능하다. 보통 X, Y를 RGB채널과 A채널로 나눈 상태로 압축한다.   

### Tangent Space Transform
Normal Map을 적용하기 위한 기준으로 Tangent가 필요하다. Tangent 벡터는 Normal에 수직인 벡터로 접선 벡터라고도한다. Normal 벡터와 Tangent 벡터를 외적하면 벡터가 하나 추가되어 3개의 벡터로 회전을 구현할 수 있다.   
오브젝트의 Normal, Tangent, 추가된 벡터를 가지는 행렬과 Normal Map을 곱해주면 최종 Normal이 나온다.

## Parallax Mapping
![Parallax_Map](/images/ParallaxMap.jpg)   
[이미지 출처](https://docs.unity3d.com/Manual/StandardShaderMaterialParameterHeightMap.html)   
Height Map을 추가로 사용해 높이값을 저장한다. 인접한 픽셀을 정보를 얻어 차폐되어 있는지 체크해 깊이감을 표현한다. 연산 비용이 비싸다.

## Displacement Mapping
실제로 폴리곤을 변형한다. 적은 폴리곤으로 요철을 표현하면 표현 가능한 크기에 한계가 있다. 따라서 테셀레이션을 사용해야 하기 때문에 하드웨어 제약이 있다.
