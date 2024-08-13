# Scriptable Object

## 개요
Scriptable Object는 대량의 데이터를 저장하기 적합한 직렬화가능한 Unity 클래스다. Scriptable Object는 Unity에디터에서 에셋으로 저장될 수 있다.

## 사용 예시
1. 경량 패턴
ScriptableObject에 공유 데이터를 넣어놓고 프리팹은 ScriptableObject를 참조하면 메모리를 절약할 수 있다.

2. DontDestoryOnLoad 대체
에셋으로 존재하는 ScriptableObject를 직접 참조하는 경우 씬이 변경되어도 그 값이 유지된다.