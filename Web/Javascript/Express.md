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

app.use((req, res)=>{
    res.status(404).send('File Not Found');
})

app.listen(port, ()=>{
    console.log(`Server running at http://localhost:${port}`);
})

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