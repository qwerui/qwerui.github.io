# AJAX

## AJAX

Javascript를 이용해 XML 등의 데이터를 서버와 비동기적으로 주고받는 기술, XMLHttpRequest 객체가 중심이된다.

- AJAX의 ReadyState

0 : UNSENT, 객체가 생성되었지만 open()이 호출되지 않음   
1 : OPENED, open()이 호출되어 요청이 초기화, send()를 호출하기 전   
2 : HEADERS_RECEIVED, send()가 호출되어 서버로 요청이 전송된 상태, 이때, 응답 헤더는 수신 상태   
3 : LOADING, 서버로부터 응답의 본문을 수신하고 있는 상태   
4 : DONE, 요청이 완료된 상태, 모든 데이터가 수신되었고, 응답이 준비됨   

### AJAX 예시

```jsx
function fetchData() {
      var input = document.getElementById("nameInput").value;
      var xhr = new XMLHttpRequest(); //XMLHttpRequest 객체 생성
      xhr.open("GET", "AJAXDatabaseServlet?name=" + input, true); //GET방식으로 URL에 접속, true부분은 비동기 여부
      xhr.onload = function() { // readyState 변화 이벤트
         if (xhr.status == 200) {
            document.getElementById("result").innerHTML = xhr.responseText;
         }
      };
      xhr.send(); //송신
   }
```

### JQuery를 사용할 경우

```jsx
$.ajax({
        url : '/ajax/sample.do',
        type : 'GET',
        dataType : "json",
        contentType:"application/json",
        data : {
        		seq : seq
        		, type : type
                },
        timeout: 10000,
        beforeSend:function(){},
        success : function(data){},
        error : function(request, status, error){ },
        complete:function(){}
});
```