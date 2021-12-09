1. `vagrant@vagrant:~$ telnet stackoverflow.com 80`
```Trying 151.101.65.69...
Connected to stackoverflow.com.
Escape character is '^]'.
GET /questions HTTP/1.0
Host: stackoverflow.com

HTTP/1.1 301 Moved Permanently
cache-control: no-cache, no-store, must-revalidate
location: https://stackoverflow.com/questions
x-request-guid: 1321d54f-710c-47de-869c-bc10cfe7091e
feature-policy: microphone 'none'; speaker 'none'
content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
Accept-Ranges: bytes
Date: Thu, 09 Dec 2021 21:13:01 GMT
Via: 1.1 varnish
Connection: close
X-Served-By: cache-hel1410029-HEL
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1639084381.362949,VS0,VE109
Vary: Fastly-SSL
X-DNS-Prefetch-Control: off
Set-Cookie: prov=a1cddc1f-fbfa-38df-a273-c9a816e1f54b; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly```
<br>
<i>301 Moved Permanently -Означает что запрошенный ресурс был на постоянной основе перемещён в новое месторасположение, и указывающий на то, что текущие ссылки, использующие данный URL, должны быть обновлены. Адрес нового месторасположения ресурса указывается в поле Location получаемого в ответ заголовка пакета протокола HTTP</i>
```

2.
`Request URL: http://stackoverflow.com/
  Request Method: GET
  Status Code: 307 Internal Redirect
  Referrer Policy: strict-origin-when-cross-origin
  Location: https://stackoverflow.com/
  Non-Authoritative-Reason: HSTS
  Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
  Upgrade-Insecure-Requests: 1
  User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.93 Safari/537.36 `
  
![11](https://github.com/a-khov/devops-netology/raw/main/Homework/3.6/8.jpg)

