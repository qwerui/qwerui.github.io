# Vault

## 개요
민감한 정보를 관리할 수 있는 Hashicorp의 툴, Spring의 application.yml을 암호화하거나 임시 ssh 비밀번호 생성 및 클라우드 인증 생성 등을 수행할 수 있다.

## Key/Value 암호화
![vault_kv](/images/vault.PNG)
<br/>

vault를 브라우저에서 접속하면 위와 같이 secret과 secret에 들어갈 값을 지정할 수 있다. 다음과 같은 key-vaule를 생성했다.

```javascript
var options = {
    apiVersion: 'v1',
    endpoint: 'http://localhost:8200',
    token: 'root1234' //vault에 접속하기 위한 토큰, 환경변수로 사용하는 것이 좋아보인다.
};

var vault = require("node-vault")(options);

async function read() {
    const result = await vault.read('secret/data/mysql-connection');
    console.log(result.data.data);
}

read();

// 결과 : { password: 'mypassword', username: 'myname' }

```