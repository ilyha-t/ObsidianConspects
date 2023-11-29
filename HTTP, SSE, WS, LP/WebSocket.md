Протокол взаимодействия в режиме real time. Подойдет, если необходимо реализовать двунаправленную постоянную передачу данных, например, чаты, игры, торговые площадки. Предоставляет `full-duplex` **двустороннюю коммуникацию** по верх `TCP`.

! Вместо стандартного HTTP используется протокол WS.
Браузер сначала делает HTTP-запрос с заголовками `Connection: Upgrade` и `Upgrade: websocket` для смены протокола на websocket. Сервер в ответ отправляет код 101 и соединение переключается на websocket.

Клиент:
```javascript
let socket = new WebSocket("wss://javascript.info/article/websocket/chat/ws");

// отправка сообщения из формы
document.forms.publish.onsubmit = function() {
  let outgoingMessage = this.message.value;

  socket.send(outgoingMessage);
  return false;
};

// получение сообщения - отобразить данные в div#messages
socket.onmessage = function(event) {
  let message = event.data;

  let messageElem = document.createElement('div');
  messageElem.textContent = message;
  document.getElementById('messages').prepend(messageElem);
}
```

Сервер:
На сервере необходимо хранить стейт клиентов и отправлять им сообщения при событии 'message':
```javascript
const ws = new require('ws');
const wss = new ws.Server({noServer: true});

const clients = new Set();

http.createServer((req, res) =&gt; {
  wss.handleUpgrade(req, req.socket, Buffer.alloc(0), onSocketConnect);
});

function onSocketConnect(ws) {
  clients.add(ws);

  ws.on('message', function(message) {
    for(let client of clients) {
      client.send(message);
    }
  });

  ws.on('close', function() {
    clients.delete(ws);
  });
}
```

**Проблемы:**
`WebSockets` имеет схожие проблемы с `Long-Polling`, так как тоже требует соответствующего сервера для поддержки большого количества соединений в памяти, а так же требует синхронизации при масштабировании сервера более чем на 1 процесс.

`WebSockets` часто блокируется балансировщиками нагрузки и файерволами, что может требовать дополнительной настройки.