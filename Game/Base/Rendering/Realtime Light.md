# Realtime Light

## Forward Rendering
전통적인 렌더링 기법, 카메라에 가까운 객체부터 렌더링해 뒤에 겹쳐지는 부분의 픽셀 연산을 건너뛸 수 있음. 픽셀 쉐이더에서 라이팅 연산.   
**Multi-Light Multi-Pass** : 빛 1개당 패스 1개 생성, 빛 * 오브젝트 수 = 드로우 콜   
**Multi-Light Single-Pass** : 빛이 여러개여도 1개 패스 생성, 라이팅 개수가 제한됨.

## Deferred Rendering
오브젝트를 먼저 연산하고 라이팅을 나중에 연산. 오브젝트의 픽셀 쉐이더까지 연산한 결과(Albedo, Depth, Normal)를 Geometry buffer에 저장한 뒤 라이팅 연산 수행. 많은 라이팅을 사용해도 적은 연산으로 수행할 수 있음. 머티리얼을 다양하게 사용할 수 없음(투명 등).   
**Light Pre Pass** : Deferred 과정의 결과를 Lit Buffer에 저장하고 다시 오브젝트를 그린 결과와 Lit Buffer의 결과를 합성해 출력. 드로우 콜이 2배가 되는 단점이 있음.   
**Inferred Lighting** : G-buffer의 사이즈를 줄이고 최종 출력에 복원해 대역폭을 확보, 퀄리티 하락의 한계가 존재.

## Tile-based GPU
**TBR** : Primitive -> Tile buffer -> Framebuffer   
**TBDR** : Vertex Shader -> Geometry Working Set(VRAM에 저장) -> Fragment Shader -> Local Tile Memory -> Framebuffer   
렌더링 기준이 되는 타일 안에서 모든 것을 처리할 수 있어야 가속을 받는다. 블러, SSAO, 굴절 등을 처리할 때 타일을 넘어가는 영역의 픽셀 정보를 얻어와야하는 경우 가속을 받지 못한다.

## Tiled Forward Shading(Forward+)
오브젝트를 타일로 쪼개 타일별로 영향을 받는 라이트를 처리. 기존 방식은 오브젝트 단위로 라이트를 처리.   
