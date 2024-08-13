# Singleton

## Singleton 개요
객체가 1개만 존재함을 보장하는 패턴, 객체에 전역적으로 접근할 수 있다.

## 코드
```
public class Singleton
{
    private static Singleton instance;
    public static Singleton Instance
    {
        get
        {
            if(instance==null)
            {
                instance = new Singleton();
            }

            return instance;
        }
    }
}
```

## 주의사항
1. 전역 접근으로 인한 결합도 증가
2. 싱글톤이 여러개일 경우 초기화 순서를 보장하기 어렵다.
3. 기본적으로 멀티스레드에 안전하지 않다. 생성 시점에서는 DCL, 이른 초기화, Lazy등 여러 기법을 사용해야하고, 메소드 단위로 따로 스레드 안전하게 구현해야하는 경우가 있다.