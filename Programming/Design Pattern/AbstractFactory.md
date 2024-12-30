# Abstract Factory

## Abstract Factory 개요
객체를 생성할 때 직접 생성자를 호출하지 않고, 객체 생성을 담당하는 팩토리를 생성해 요청하는 방법. 객체를 생성하는 기능만 모아놓을 수 있고, 클래스 타입을 정확히 몰라도 클래스 계층에 맞게 객체를 생성할 수 있다.

## 생성 코드
```
public interface IFactory
{
    TopObject Create();
}

public class ConcreteFactory : IFactory
{
    TopObject Create()
    {
        // 객체 생성 코드
    }
}
```

## 사용 코드
```
IFactory factory = new ConcreteFactory();
TopObject obj = Create();
```