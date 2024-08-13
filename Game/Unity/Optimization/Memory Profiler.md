# Memory Profiler

![Memory_Profiler_Editor](/images/MemoryProfiler.PNG)
Package Manager에서 Memory Profiler를 설치하면 이런 화면이 나온다.   

**UI 해설**   
Snapshot : 스냅샷을 찍는다.   
Editor(변동 가능) : 스냅샷을 찍을 대상을 선택한다.   
Single Snapshot : 단일 스냅샷을 분석한다.   
Compare Snapshots : 두 개의 스냅샷을 비교한다.   
Summary : 메모리의 사용량 요약   
Unity Objects : Unity Object에 의한 메모리 사용량, 주로 이쪽을 보면 된다.   
All of Memory : 모든 메모리   

**주의 사항**   
1.에디터에서 체크하기보다는 빌드한 뒤 타깃 디바이스에서 체크하는 것이 옳다.   
   
![Memory_Compare](/images/MemoryCompare.PNG)
위가 에디터의 Play Mode, 아래가 빌드한 프로젝트를 스냅샷한 결과다.   
간단한 프로젝트임에도 상당한 차이가 발생한다는 것을 알 수 있다.   
   
2.Development Build를 체크하고 빌드해야한다. 그렇지않으면 프로파일러가 인식을 하지 못한다.   
3.스크립트를 통해서 스냅샷을 찍을 수 있다. MemoryProfiler.TakeSnapshot(string, Action<string, bool>)을 사용하면 된다.   
string : 스냅샷을 저장할 .snap을 포함하는 파일 경로   
Action<string, bool> : 스냅샷 종료 콜백 string=path, bool=success   
+어째선지 스크립트로 스냅샷을 찍으면 찍혔을 때 화면이 나오지 않고 검은 화면이 나온다.