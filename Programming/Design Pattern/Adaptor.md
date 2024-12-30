# Adaptor

## Adaptor 개요
호환되지 않는 인터페이스를 연결할 때 사용

## 코드
```
public class Target
{
    public void Run(){}
}

public interface IClient
{
    void Do();
}

public class Adaptor : IClient
{
    private Target target;

    Adaptor(Target target) => this.target = target;
    public void Do() => target.Run();
}

```