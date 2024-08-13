# SignalR

## 개요
실시간 사용자 상호작용 및 데이터 갱신을 위한 오픈소스 라이브러리. 웹 소켓을 지원하며 웹 소켓이 지원되지 않는 브라우저에서는 Long Polling(클라이언트가 서버에 지속적으로 요청을 보내고, 서버는 이 요청에 대해 새로운 데이터가 있을 때까지 응답을 보류) 같은 기술로 대체한다.

## 기본 세팅
```c#
//루트에 생성
    public class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            app.MapSignalR();
        }
    }
```

## Hub 클래스
```c#
//기본 형식
    [HubName("chat")]
    public class ChatHub : Hub
    {
        public void ClientToServer(string msg)
        {
            Clients.All.serverToClient(msg);
        }
    }
```

### 메시지 전달 방식
아래 속성들에 .메소드명(값)을 붙여야한다.  
- Clients.All : 전체 전송
- Clients.Caller : 호출자 전송
- Clients.Others : 피호출자 전송
- Clients.Group(그룹이름) : 그룹 전송
- Clients.Client(커넥션ID) : 특정 사용자 전송

## 클라이언트
```html
<!-- 다음과 같은 스크립트를 선언해야한다 -->
        <script src="/Scripts/jquery-3.4.1.min.js"></script>
        <script src="/Scripts/jquery.signalR-2.4.3.min.js"></script>
        <script src="/signalr/hubs"></script>
        <script>
    $(function () {
        var chat = $.connection.허브이름;
        chat.client.메소드명[Clients.에서 지정한 이름] = function(){}; 
        $.connection.hub.start().done(function (){
                chat.server.메소드명[클래스의 메소드명](넘길 값);
        });
    });
</script>
```