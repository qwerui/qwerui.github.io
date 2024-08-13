# Observer

## Observer 개요
상태의 변화를 감시하는 Observer가 상태가 변화하는 Observable의 변화를 감지한다. 1:다 관계에서 의존성을 줄이는 방법 중 하나다. C#은 delegate를 통해 구현할 수도 있다.

## 코드
```
public interface IObserver
{
    void Notify();
}

public class Observable
{
    List<IObserver> observers = new();

    public void AddObserver(IObserver observer) => observers.Add(observer);
    public void RemoveObserver(IObserver observer) => observers.Remove(observer);

    public void SomethingChanged()
    {
        for(var observer : observers)
        {
            observer.Notify();
        }
    }
}
```