# Socket

## socket.io
```javascript
const express = require('express');
const http = require('http');
const socketio = require('socket.io');

var app = express();
var server = http.createServer(app);
const io = socketio(server);

// 소켓은 on으로 이벤트를 리스닝하고
// emit으로 이벤트를 발생시킨다.
io.sockets.on('connection'(socket)=>{
    socket.emit('message', '메시지');
})

```