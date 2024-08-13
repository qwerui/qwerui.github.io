# Chain of Responsibility

## 책임 사슬 개요
특정한 동작을 여러 객체가 엮여서 처리할 때 사용. 주로 이벤트 핸들러를 구현할 때 사용한다.

## 코드
```
public class Process
{
    private Process next;
    public void SetNext(Process next) => this.next = next;
    public void Run(bool someCheck)
    {
        if(someCheck)
        {
            //Process
        }
        else
        {
            //Process
            next.Run();
        }
    }
}

//상속을 통한 구현도 가능하다
public class Parent
{
    public virtual void Run(bool someCheck)
    {
        //SomeProcess
    }
}

public class Child : Parent
{
    public virtual void Run(bool someCheck)
    {
        if(someCheck)
        {
            //Process
        }
        else
        {
            base.Run(someCheck);
        }
    }
}
```