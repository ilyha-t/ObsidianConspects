Это процесс, при котором соединение между сервером и клиентом не разрывается длительный период, а при разрыве повторно создается. Такой механизм позволяет передавать промежутками различные данные. Например, он неплох для отправки статусов бизнес-сущностей клиенту или любой другой real-time информации. В отличие от обычных запросов, здесь создается всего одно соединение, что уменьшает нагрузку на сервер и ускоряет передачу.

Работает Long Polling так:

1. Клиент делает запрос
2. Сервер принимает запрос и не отвечает до тех пор, пока ему не будет чем ответить.
3. Клиент получает ответ и заново делает запрос.

Это уже настоящий `Real-time`. Здесь мы **не делаем лишних запросов**, но **не каждый веб-сервер сможет поддерживать большое количество запросов в памяти одновременно**.