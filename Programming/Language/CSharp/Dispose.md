# Dispose

## Dispose 개요
**관리되지 않는 리소스** : 파일, 창, 네트워크 연결 또는 데이터베이스 연결 등의 운영 체제 리소스를 래핑하는 개체   
주로 관리되지 않는 리소스를 해제하는데 사용한다. IDisposable를 구현한 뒤 Dispose를 호출하거나 using 키워드에서 사용해 using의 영역을 벗어나면 자동으로 Dispose를 호출하도록 사용하면 된다.

## Dispose Pattern

Dispose을 통해 수동으로 리소스를 해제할 수 있게하고, GC.SupressFinalize를 통해 GC에게 개체가 수동으로 삭제되어 더 이상 종료할 필요가 없음을 알린다.   

**기본 코드**
```
public class DisposableResourceHolder : IDisposable {

    private SafeHandle resource; // handle to a resource

    public DisposableResourceHolder() {
        this.resource = ... // allocates the resource
    }

    public void Dispose() {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    protected virtual void Dispose(bool disposing) {
        if (disposing) {
            if (resource!= null) resource.Dispose();
        }
    }
}
```

### 주의사항
1. Dispose(bool)은 Dispose()에서 true, 소멸자에서 false로 호출된다. 관리되지 않는 리소스는 bool값에 상관없이 처리해야하고, 관리되는 리소스는 true일때만 처리해야한다.
2. Dispose는 예외를 throw하지 않고 여러번 호출할 수 있어야한다.
