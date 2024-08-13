# Test Runner

## Test Runner 개요
Unity에서 Play Mode, Edit Mode, 타겟 디바이스에서 유닛 테스트를 할 수 있는 모듈. nUnit을 기반으로 한다.   
**Play Mode** : 씬을 플레이해 테스트를 진행한다. 런타임을 대상으로 하는 테스트.   
**Edit Mode** : 에디터에서 바로 테스트를 진행한다. 게임 코드와 에디터 코드에 모두 액세스 할 수 있다.

## Test Runner 사용법
![TestRunner_Editor](/images/TestRunnerEditor.PNG)   
1. Window -> General -> Test Runner
2. Play/Edit Mode 선택 후 Create Folder 및 Create Script, 테스트 용 폴더 안에 Assembly Definition과 스크립트가 생성된다.
3. 생성된 Assembly Definition에 테스트를 진행할 로직이 있는 스크립트의 Assembly Definition Reference를 추가한다.
4. 테스트 코드 작성
5. Test Runner 창에서 테스트 할 테스트 함수를 지정하고 실행한다. 전부를 실행할 수도 있다.

## 테스트 코드 작성법
```
public class NewTestScript
{
    [Test]
    public void NewTestScriptSimplePasses()
    {
        var obj = new Counter();
        obj.AddOne();
        Assert.AreEqual(1, obj.value);
    }
    [UnityTest]
    public IEnumerator NewTestScriptWithEnumeratorPasses()
    {
        var obj = new GameObject();
        var mover = obj.AddComponent<CubeMover>();
        mover.MoveRight();
        yield return null;
        Assert.AreEqual(new Vector3(1, 0, 0), obj.transform.position);
    }
}
```
테스트 스크립트의 함수에 Test 또는 UnityTest 어트리뷰트를 지정하면 Test Runner에서 감지할 수 있다. 내부의 Assert의 결과에 의해 성공/실패 여부가 결정된다.   
```
static int[] values = new int[] { 1, 5, 6 };

[UnityTest]
public IEnumerator MyTestWithMultipleValues([ValueSource(nameof(values))] int value)
{
    yield return null;
}
```
파라미터화 테스트를 실행할 수 있다. 위 코드에선 1, 5, 6의 값이 테스트 코드에 입력되어 3번 테스트를 진행한다. UnityTest에서는 ValueSource만 지원한다.   

## 상세 어트리뷰트나 메소드 링크
[Unity Test Framework](https://docs.unity3d.com/kr/Packages/com.unity.test-framework@2.0/manual/index.html)   
[nUnit.Framework](https://docs.nunit.org/api/NUnit.Framework.html)
