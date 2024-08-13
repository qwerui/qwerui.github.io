# Decorator

## Decorator 개요
객체를 유지하면서 작업을 추가하기 위해 사용, 래퍼라고도 한다. 추가 기능이 접근 제어가 목적이면 Proxy 패턴으로 분류한다.

## 코드
```
public interface IProcess
{
    void Run();
}

public class Inner : IProcess
{
    public void Run()
    {
        //Some Process
    }
}

public class Outer : IProcess
{
    IProcess inner;
    
    Outer(IProcess inner)=>this.inner = inner;

    public void Run()
    {
        //Some Addtional Process
        inner.Run();
    }
}
```