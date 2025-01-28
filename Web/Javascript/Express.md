# Express

## Http Web Server
```javascript
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res)=>{
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello World\n');
});

server.listen(port, hostname, ()=>{
    console.log(`Server running at http://${hostname}:${port}/`);
});

```

## Express
http 모듈에 많은 기능을 포함하는 모듈, 뷰 템플릿을 지원한다.   
express-generator를 이용하면 router, view, model, 필수 미들웨어, 에러 미들웨어등 서버를 만들 때 마다 해야하는 작업을 자동으로 생성해준다.

```javascript
// 기본 서버 구축

const express = require('express');
const app = express();
const port = 3000;

//app.get의 첫 번째 매개변수는 url이다 :이름으로 지정하면 req.params.name으로 파라미터를 받을 수 있다.
app.get('/', (req, res)=>{
    res.send('Hello World!');
});

// app.use만으로 로그를 찍어보면 2번 찍히는 경우가 있다. 원인은 브라우저가 favicon.ico를 같이 요청하기 때문. 이에 대한 처리도 당연히 필요하다.
app.use((req, res)=>{
    res.status(404).send('File Not Found');
})

app.listen(port, ()=>{
    console.log(`Server running at http://localhost:${port}`);
})

```

### 오류 처리
```javascript
app.use((req, res, next)=>{
    throw new Error("에러 발생 시 에러 처리 코드가 받는다.");
});

app.use((req, res, next)=>{
    next(new Error("이 방법도 가능하다"));
});

app.use((err, req, res, next)=>{
    // 에러처리 코드
    // 비즈니스로직을 작성하고 맨 마지막에 작성해야 오류처리가 가능하다.
});
```

### Router
1. Router()를 이용해 라우터를 모듈로 만든다.
2. 해당 모듈은 1차적으로 걸러진 라우터에서 일치하는 URL과 메소드에 따라 실행
3. app.use로 등록한 endpoint에 따라 실행되는 라우터를 만든다.

```javascript
// app.js
const express = require('express');
const userRouter = require('./routes/user');

const app = express();

app.use('/user', userRouter);

module.exports = app;

// ./routes/user.js
const express = require('express');
const router = express.Router();

router.get('/', (req, res, next)=>{
    res.send("Hello, User!!!");
});

module.exports = router;

```

### Response
- send() : 전송된 데이터에 따라 알맞은 형식으로 변환되어 전송
- download() : 해당 파일 다운로드
- redirect() : 해당 경로 이동
- json() : json형태로 데이터 응답
- render() : 뷰 템플릿 엔진을 HTML로 렌더링
- status() : 상태코드 변경

### 주요 모듈
[많이 사용하는 모듈](https://expressjs.com/ko/resources/middleware.html)

- express-validator : querystring, body 유효성 검증
- jsonwebtoken : JWT 생성 및 검증
- pino, winston : 로깅
- opossum : 서킷브레이커
- bottleneck : API 요청 수 제한
- i18next : 다국어 처리