# Database

## MySQL
```javascript
// MySQL 연동 코드
const mysql = require("mysql");
const conn_info = {
    host : "localhost",
    port : "3306",
    user : "사용자",
    password : "비밀번호",
    database : "데이터베이스"
}

//커넥션
const conn = mysql.createConnection(conn_info);
conn.connect((error)=>{
    if(error){
        console.log("접속 오류");
    } else {
        console.log("접속 성공");
    }
});

//커넥션 풀
const pool = mysql.createPool({
    connectionLimit : 10,
    host : "localhost",
    port : "3306",
    user : "사용자",
    password : "비밀번호",
    database : "데이터베이스"
});

pool.getConnection((error, conn)=>{
    if(error){
        //에러 처리
    }
    //DB 작업
    
    conn.release();//반환
});

// 쿼리
conn.query(sql, (error, rows)=>{}); //SELECT
conn.query(sql, (error)=>{}); //INSERT, UPDATE, DELETE 
```

## MongoDB
```javascript
const MongoClient = require("mongodb").MongoClient;
var url = 'mongodb://localhost:27017/database';

MongoClient.connect(url, (err, db)=>{
    //DB 작업
    var obj = db.collection('컬렉션 이름');
});

// CRUD
obj.insert().then((results)=>{}); //입력
obj.find().toArray((err, docs)=>{}); //다수 조회
obj.findOne(); //단일 조회
obj.update().then((err)=>{}); //다수 수정
obj.updateOne().then((err)=>{}); //단일 수정
obj.deleteOne().then((err, result)=>{}); //단일 삭제
obj.deleteMany().then((result)=>{}); //다수 삭제

```

### Mongoose
```javascript

//ODM(객체-도큐먼트 매퍼)을 사용하는 MongoDB용 툴

const mongoose = require('mongoose');
const url = 'mongodb://localhost:27017/database';
mongoose.connect(url); //데이터베이스 연결

const db = mongoose.connection;

db.on('error', (err)=>{
    //에러처리
});
db.on('open', ()=>{
    //연결처리
});

var Scheme = mongoose.Schema({
    //스키마 내용
});

var Model = mongoose.model('Collection', Scheme);

var Data = new Model({name:value});
// 삽입
Data.save().then((product)=>{
    //성공 코드
},
(err)=>{
    //실패 코드
});

Model.create({}); // 모델에서 객체 생성 없이 삽입

// 조회
Model.find();
Model.findById();
Model.findOne();

// 수정
Model.update({}, {});

// 삭제
Model.delete({});
```