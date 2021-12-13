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
`Request URL: http://stackoverflow.com/`
  Request Method: GET <br>
  Status Code: 307 Internal Redirect<br>
  Referrer Policy: strict-origin-when-cross-origin<br>
  Location: https://stackoverflow.com/<br>
  Non-Authoritative-Reason: HSTS<br>
  Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9<br>
  Upgrade-Insecure-Requests: 1<br>
  User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.93 Safari/537.36 <br>
  
![11](https://github.com/a-khov/devops-netology/raw/main/Homework/3.6/8.jpg)

Дольше всего заргужался javascript.

3. `curl ifconfig.me/ip` 
    <i>94.25.229.162</i>

4.  descr:          SPb Scartel User Network<br>
    origin:         AS47395

5. Получилось, но все скрыты 
`vagrant@vagrant:~$ sudo traceroute -nA 8.8.8.8 -I`
```traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  10.0.2.2 [*]  0.263 ms  0.239 ms  0.226 ms
 2  10.225.40.1 [*]  6.012 ms  6.127 ms  6.102 ms
 3  * * *
 4  * * *
 5  * * *
 6  * * *
 7  * * *
 8  * * *
 9  * * *
10  * * *
11  * * *
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  * 8.8.8.8 [AS15169]  7.026 ms * ```

6. `vagrant@vagrant:~$ mtr 8.8.8.8 -z` 
```
                                                 My traceroute  [v0.93]
vagrant (10.0.2.15)                                                                            2021-12-10T11:57:50+0000
Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                                                               Packets               Pings
 Host                                                                        Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. AS???    _gateway                                                         0.0%     9    0.4   0.3   0.2   0.4   0.1
 2. AS???    192.168.156.82                                                   0.0%     9    3.5   4.1   3.2   5.2   0.8
 3. (waiting for reply)
 4. (waiting for reply)
 5. (waiting for reply)
 6. (waiting for reply)
 7. (waiting for reply)
 8. (waiting for reply)
 9. AS31133  37.29.23.69                                                      0.0%     9   27.2  33.8  23.8  53.6   9.1
10. AS31133  37.29.23.68                                                      0.0%     9   30.8  38.3  29.5  62.7  10.3
11. AS31133  37.29.17.87                                                      0.0%     9   39.6  35.0  23.8  51.9   8.8
12. AS15169  74.125.244.181                                                   0.0%     9   61.1  37.9  26.8  61.1   9.8
13. AS15169  142.251.51.187                                                   0.0%     9   35.3  39.1  33.2  46.0   4.6
14. AS15169  172.253.51.245                                                   0.0%     9   40.7  37.8  32.2  43.7   3.6
15. (waiting for reply)  ```

Самая большая задержка на AS15169 74.125.244.181 

7. `vagrant@vagrant:~$ dig +trace dns.google`
<details>   
<summary>
; <<>> DiG 9.16.1-Ubuntu <<>> +trace dns.google </summary>
;; global options: +cmd
.                       20370   IN      NS      g.root-servers.net.
.                       20370   IN      NS      c.root-servers.net.
.                       20370   IN      NS      e.root-servers.net.
.                       20370   IN      NS      i.root-servers.net.
.                       20370   IN      NS      l.root-servers.net.
.                       20370   IN      NS      a.root-servers.net.
.                       20370   IN      NS      k.root-servers.net.
.                       20370   IN      NS      d.root-servers.net.
.                       20370   IN      NS      h.root-servers.net.
.                       20370   IN      NS      f.root-servers.net.
.                       20370   IN      NS      b.root-servers.net.
.                       20370   IN      NS      m.root-servers.net.
.                       20370   IN      NS      j.root-servers.net.
;; Received 262 bytes from 127.0.0.53#53(127.0.0.53) in 83 ms

google.                 172800  IN      NS      ns-tld1.charlestonroadregistry.com.
google.                 172800  IN      NS      ns-tld2.charlestonroadregistry.com.
google.                 172800  IN      NS      ns-tld3.charlestonroadregistry.com.
google.                 172800  IN      NS      ns-tld4.charlestonroadregistry.com.
google.                 172800  IN      NS      ns-tld5.charlestonroadregistry.com.
google.                 86400   IN      DS      6125 8 2 80F8B78D23107153578BAD3800E9543500474E5C30C29698B40A3DB2 3ED9DA9F
google.                 86400   IN      RRSIG   DS 8 1 86400 20211223050000 20211210040000 14748 . FhODHx8dKMPVhrDltsIc1q188oiBGogmzjKPLlEKED5AgFYZwTzFc6/V YAFRkwYGRxwvhYGVYeREH7ut+jNRl+8SC61kgG8wG+wICmsfSYI161k4 6g2rVTFWTjqpwF7hsqUu1udShiVLR7Nz8cfkm5Xc5KZdg2HjvaeDyGGY m9EaDqL6k08MuYoVNgdzgLuOoUaMCFf9kFAgY6ojd913yzS0TrLBeiCy dxLgyYdJAqvGha07Q2EnCR1EcNK+eanpdmLel7erk68b/c63B8a9nYcq I4qoyctEUb1vfdZ6wrEQu6ARm8xwfelKxhFEWqCVCNIkg1z7YgvR/t42 /8w6Bw==
;; Received 730 bytes from 198.97.190.53#53(h.root-servers.net) in 55 ms

dns.google.             10800   IN      NS      ns2.zdns.google.
dns.google.             10800   IN      NS      ns3.zdns.google.
dns.google.             10800   IN      NS      ns4.zdns.google.
dns.google.             10800   IN      NS      ns1.zdns.google.
dns.google.             3600    IN      DS      56044 8 2 1B0A7E90AA6B1AC65AA5B573EFC44ABF6CB2559444251B997103D2E4 0C351B08
dns.google.             3600    IN      RRSIG   DS 8 2 3600 20211229191018 20211207191018 44925 google. dIBP1HqZEqFu9MVTeX1By5oNiRH7Y294ewubQW6VTa9r4CamMzgVA7Py h1yfqtO47xw3VPclaleT1V5vxzzPMQQg2Q/BZvL6TnqWSUreNItCCuHU bSGklBDfTn3VdBkf5+0OJ2hs9Q515BSyRSAp0MJEB75EFmE9aWAZXdAh gnU=
;; Received 506 bytes from 216.239.34.105#53(ns-tld2.charlestonroadregistry.com) in 103 ms

dns.google.             900     IN      A       8.8.4.4
dns.google.             900     IN      A       8.8.8.8
dns.google.             900     IN      RRSIG   A 8 2 900 20220109081006 20211210081006 1773 dns.google. Q8EMwf3HFBJawtJGRLi6M5/GIkaqBu00836UOMskbXALZnCw2/ElTDIL 4GjuRRIkRXdBo+zJGplaWLjKJO8J9cRdB8pKwNbCiObO80CDaWL9HVjU fU21QP3IScEbkdaodtyzhb7qckvM4oQVMh6tJwZJa0R7xs7RShaOrERv vgI=
;; Received 241 bytes from 216.239.34.114#53(ns2.zdns.google) in 99 ms </details>

dns.google A-записи прописаны ip 8.8.4.4 и 8.8.8.8  
Серверы ns1.zdns.google. ns2.zdns.google. ns3.zdns.google. ns4.zdns.google. 

8. `dig -x 8.8.8.8` <br>
`;; ANSWER SECTION:` <br>
`8.8.8.8.in-addr.arpa.   21952   IN      PTR     dns.google.` <br>