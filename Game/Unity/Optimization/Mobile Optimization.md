# Mobile Optimization

Unity Korea 채널의 영상을 보고 정리한 내용이다.
[모바일최적화2022LTS_1](https://www.youtube.com/watch?v=-IrpQzZi_p8)   
[모바일최적화2022LTS_2](https://www.youtube.com/watch?v=6dBsTYa3APA)

### Unity의 Profiling
Unity의 프로파일러는 모바일 환경에서 GPU 항목이 부정확할 수 있어, Xcode나 Arm Mobile Studio 등으로 확인하는 것이 더 정확할 수 있다.    
모바일 환경에서 온도가 올라가면 스로틀링으로 인한 성능저하가 발생할 수 있다. 프로파일링 할 때 작동 시간도 고려해야한다.   
Gfx.WaitforCommands : 메인 스레드 작업 시간 > 렌더 스레드 작업 시간   
Gfx.WaitForPresent : GPU 작업 시간 > 렌더 스레드 작업 시간   
모바일 환경은 Unified Memory Architecture인데다, 가상 메모리가 없거나 PC와는 다르게 동작하기 때문에, 메모리를 효율적으로 사용해야한다.

### Profiler Marker
코드 단위로 명시적으로 프로파일링 가능, 범위는 ProfilerMarker 변수의 Begin에서 End까지 혹은 Auto를 사용하면 using 블록의 내부범위   
[ProfilerMarker_API](https://docs.unity3d.com/ScriptReference/Unity.Profiling.ProfilerMarker.html)

### Adaptive Performance
활성화시 아래 영역을 감지한다.
1. 디바이스 온도
2. 스로틀링 이벤트 발생
3. CPU, GPU bound
4. 이전 프레임을 기반으로한 프레임 예상

### 기타 사용 가능한 최적화
1. 매 프레임마다 갱신할 필요가 없는 코드는 interval을 준다.
2. 빈 UnityEvent를 제거한다.
3. 빌드 전에 Debug를 제거한다.
4. string 매개변수보다 해시 값을 사용한다.
5. 적절한 자료구조 사용
6. 런타임에서 컴포넌트 추가 지양
7. 오브젝트와 컴포넌트를 캐싱한다.
8. 오브젝트 풀 사용
9. ScriptableObject 사용을 통한 경량 패턴(프리팹에 직접 값을 담을 경우 Instantiate 할 때마다 복사본이 생성된다.)
10. Accelerometer Frequency를 낮추거나 비활성화.
11. 불필요한 Player 또는 Quality 설정 비활성화.
12. 불필요한 물리 비활성화
13. 적절한 프레임 설정
14. 거대 계층구조 지양
15. Transform 연산을 한 번에 수행한다. SetPositionAndRotation 권장
16. Vsync 활성화