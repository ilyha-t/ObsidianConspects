### Polling (Short-polling)

Данный способ для справки, он уже не применяется!
`Polling` подразумевает **опрос сервера на наличие новых данных с определенным интервалом**. 

- Для опроса используется `XMLHttpRequest` или `fetch`
- Клиент посылает запрос каждые `n` секунд
- Сервер отвечает как обычно

Этот подход рабочий, хотя его **основным недостатоком является чрезмерное расходование ресурсов сервера**.

Для каждого запроса нужно

- Установить `TCP` соединение (исключение `Keep-Alive`) и передать заголовки запроса
- Сделать запрос в базу данных
- Вернуть данные (чаще всего устаревшие, так как новых еще не было) и закрыть соединение

Это не настоящий `Real-time`, так как **при появлении новых данных, они приходили не раньше, чем следующий “тик” интервала**.