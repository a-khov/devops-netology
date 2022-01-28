UFW уже предустановлен, однако sudo apt install ufw для установки и sudo ufw enable для включения


```
ssh 192.168.56.1 -l hello 
# где -l ключ для логина

```

Открытие портов 
```

sudo ufw allow 443/tcp
sudo ufw allow 80/tcp
sudo ufw allow 22/tcp

```

установка vault

```
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -

sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"

$ sudo apt-get update && sudo apt-get install vault

```

Запускаем сервер vault
```

vault server -dev -dev-root-token-id root

```

Открываем новое окно терминала и в нём работаем с сервером vault. В нём задаём переменные VAULT_ADDR и VAULT_TOKEN

```

export VAULT_ADDR=http://127.0.0.1:8200
export VAULT_TOKEN=root

```

***
Приступаем к генерации сертификата

Включаем PKI

>vault secrets enable pki
>>>Success! Enabled the pki secrets engine at: pki/


Задаём TTL
```
vault secrets tune -max-lease-ttl=87600h pki
```

Генерируем корневой сертификат

```
vault write -field=certificate pki/root/generate/internal \
     common_name="example.com" \
     ttl=87600h > CA_cert.crt

```

Прописываем ссылки для сертификата

```

$ vault write pki/config/urls \
>      issuing_certificates="$VAULT_ADDR/v1/pki/ca" \
>      crl_distribution_points="$VAULT_ADDR/v1/pki/crl"
>>>Success! Data written to: pki/config/urls

```

***
Генерируем промежуточный сертификат

Прописываем путь к <b>pki_int</b>

```
>vault secrets enable -path=pki_int pki
>>>Success! Enabled the pki secrets engine at: pki_int/

```
Задаём время жизни

```

>vault secrets tune -max-lease-ttl=43800h pki_int
>>>Success! Tuned the secrets engine at: pki_int/

```

Вводим команду для генерации промежуточного сертификата и сохраняем его в CSR.

```

vault write -format=json pki_int/intermediate/generate/internal \
     common_name="example.com Intermediate Authority" \
     | jq -r '.data.csr' > pki_intermediate.csr

```

Теперь подпишем корневым сертификатом промежуточный

```
vault write -format=json pki/root/sign-intermediate csr=@pki_intermediate.csr \
     format=pem_bundle ttl="43800h" \
     | jq -r '.data.certificate' > intermediate.cert.pem

```

Помещаем наш новый сертификат в хранилище

```

> vault write pki_int/intermediate/set-signed certificate=@intermediate.cert.pem
>>> Success! Data written to: pki_int/intermediate/set-signed

```
Создаём роль для нашего сертификата с временем до отзыва в месяц (720 часов)

```

> vault write pki_int/roles/example-dot-com \
>      allowed_domains="example.com" \
>      allow_subdomains=true \
>      max_ttl="720h"
>>>Success! Data written to: pki_int/roles/example-dot-com

```
***

Строим цепочку сертификата для сайта

<details> 

 <summary>vault write pki_int/issue/example-dot-com common_name="test.example.com" ttl="720h"</summary>
Key                 Value
---                 -----
ca_chain            [-----BEGIN CERTIFICATE-----
MIIDpjCCAo6gAwIBAgIUSHDqInISl399Gyf02fbMe2fAbX4wDQYJKoZIhvcNAQEL
BQAwFjEUMBIGA1UEAxMLZXhhbXBsZS5jb20wHhcNMjIwMTI4MjAzNzU2WhcNMjIw
MzAxMjAzODI2WjAtMSswKQYDVQQDEyJleGFtcGxlLmNvbSBJbnRlcm1lZGlhdGUg
QXV0aG9yaXR5MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAm/DXJOg2
0BAgfUup+NwOKWRT5Saufh3LR1YnVo0j1rG7NdpwkrbLesViPBvyCamdLJY+G00a
Ogho1LUhcaewbuZROR7s7kQcRWVcZv1Rhw/qStQeNQqhPGkNY34NHVY2xrY7nZ3k
+W+1I0vLDQ6sKhnvIc4k3RC6x6q+ZOn1SXSK4Cgt5h8UgYCNEDhTzo+2Xoyb96Oc
C/lAsxi5fdh3M0Ev3RSOJRdeFf7I8kYXhVXmQMuQY2vZNHAZubef1EelOAaRaDtM
FI2z5QZR8ITQ/TDMp0dxktKtQe1WcQgTMFv0vHGXhyTP2cu8ve5qUCeu14mVjBaN
XrtJ1P8U69yN6wIDAQABo4HUMIHRMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMBAf8E
BTADAQH/MB0GA1UdDgQWBBR0B3DnH5g0XJfdD3yGvbkMupxoZzAfBgNVHSMEGDAW
gBSJIWHuACJPOnj9AD0VqUN5XI8vFTA7BggrBgEFBQcBAQQvMC0wKwYIKwYBBQUH
MAKGH2h0dHA6Ly8xMjcuMC4wLjE6ODIwMC92MS9wa2kvY2EwMQYDVR0fBCowKDAm
oCSgIoYgaHR0cDovLzEyNy4wLjAuMTo4MjAwL3YxL3BraS9jcmwwDQYJKoZIhvcN
AQELBQADggEBAEAgIWeYvHgXir6jzmC0TIqowCKUIKZ90LWE1g7P5Hi9y7mkpJvA
MB+NSFL4nYdrEgL7KIgQNqqS9hWakpc38ALTN8L/eGa1+n+N0h8elIVuVTwnjf3p
EiuangEWLMCWlKgwHJgJZif9fobcmP7vBddNsKWYeAVeoEICZ2uMISHuVG1ZOmrU
fMqdVDNUD9Nm/n5lZDAP/yxLnkr2DihyW5UQWAF6SMgSvUODSzJy/okrWRF6cMCs
3/5oF601ZH46UAiWuxORKyXvO6EjeS5Hi0uc9pixspyIbMxVD6nlS7Ze7qTHqjbx
/86VSiz27XgIOYfUIN0/S4fYNf33DoRkbY0=
-----END CERTIFICATE-----]
certificate         -----BEGIN CERTIFICATE-----
MIIDZjCCAk6gAwIBAgIUH0vnwKYLgBAEz6Eciif/8qbV/qQwDQYJKoZIhvcNAQEL
BQAwLTErMCkGA1UEAxMiZXhhbXBsZS5jb20gSW50ZXJtZWRpYXRlIEF1dGhvcml0
eTAeFw0yMjAxMjgyMDUxMDdaFw0yMjAyMjcyMDUxMzdaMBsxGTAXBgNVBAMTEHRl
c3QuZXhhbXBsZS5jb20wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDd
wLwtYmab3ZIdLGHuG7xI4+7gy7p3qwVaLGyFI6W1cG6+6DMFIbiLSes7pckgO/qr
tVFc4Gg2+5XAs7WLm08LcokCPsjD50rRKo8uc5puWtTAOw0+1kePkRh1rpz3yGTF
YDwY0Mengy2TnxqpHz7nQOavspEEvJQto9uAqw7RwwSc2qQLIMdYxyvSSeilkIj9
NOGOLvyZBoQHu2KfGHN3paPHQfM+I3Zj5uxPrNXHLERJPPqO9YwEKpXHwGBW1yQ7
PHNUQGhLIO+D3a4dmbzAKw6x2l4WGptTYa8uimxXVBID+DjAj1VLn2EPgaesIK5k
NGFS5n4wKdtu4oc6YVGVAgMBAAGjgY8wgYwwDgYDVR0PAQH/BAQDAgOoMB0GA1Ud
JQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAdBgNVHQ4EFgQUUMvDYCMsus51zUA7
KVnQvHhAl84wHwYDVR0jBBgwFoAUdAdw5x+YNFyX3Q98hr25DLqcaGcwGwYDVR0R
BBQwEoIQdGVzdC5leGFtcGxlLmNvbTANBgkqhkiG9w0BAQsFAAOCAQEAZaT32Fwo
P+Dkt1KvUq6Nfxpopd16admaHsU3fwVzra3VVSABZ9EX6R28qqmLd2nnqE+cVkKg
ASDgdGCjCWLxRoM2vrdtPhPekvoe21wwaXiafsi71oDenCBmKl2NXoyGYY4Q9ls5
mi5K8H4CZjBh4KZcrVdukktegIE49JgyEItLnO0W8mZl2rp6HmM2HYSwv3WreXdO
zncdFlmOEwSe5UeG7vGUPsF7VRhoNfunb5XHBIgVSX9zz6PCteBtNf9RSO7PEjHi
TxnQ1pd0V8CSycfCjzTrrIxjaT5BYzrNdRQ4gkdX7s0zPyrHhmSCNsywdZ2Zgn/P
UWVS99c/zZWeoA==
-----END CERTIFICATE-----
expiration          1645995097
issuing_ca          -----BEGIN CERTIFICATE-----
MIIDpjCCAo6gAwIBAgIUSHDqInISl399Gyf02fbMe2fAbX4wDQYJKoZIhvcNAQEL
BQAwFjEUMBIGA1UEAxMLZXhhbXBsZS5jb20wHhcNMjIwMTI4MjAzNzU2WhcNMjIw
MzAxMjAzODI2WjAtMSswKQYDVQQDEyJleGFtcGxlLmNvbSBJbnRlcm1lZGlhdGUg
QXV0aG9yaXR5MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAm/DXJOg2
0BAgfUup+NwOKWRT5Saufh3LR1YnVo0j1rG7NdpwkrbLesViPBvyCamdLJY+G00a
Ogho1LUhcaewbuZROR7s7kQcRWVcZv1Rhw/qStQeNQqhPGkNY34NHVY2xrY7nZ3k
+W+1I0vLDQ6sKhnvIc4k3RC6x6q+ZOn1SXSK4Cgt5h8UgYCNEDhTzo+2Xoyb96Oc
C/lAsxi5fdh3M0Ev3RSOJRdeFf7I8kYXhVXmQMuQY2vZNHAZubef1EelOAaRaDtM
FI2z5QZR8ITQ/TDMp0dxktKtQe1WcQgTMFv0vHGXhyTP2cu8ve5qUCeu14mVjBaN
XrtJ1P8U69yN6wIDAQABo4HUMIHRMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMBAf8E
BTADAQH/MB0GA1UdDgQWBBR0B3DnH5g0XJfdD3yGvbkMupxoZzAfBgNVHSMEGDAW
gBSJIWHuACJPOnj9AD0VqUN5XI8vFTA7BggrBgEFBQcBAQQvMC0wKwYIKwYBBQUH
MAKGH2h0dHA6Ly8xMjcuMC4wLjE6ODIwMC92MS9wa2kvY2EwMQYDVR0fBCowKDAm
oCSgIoYgaHR0cDovLzEyNy4wLjAuMTo4MjAwL3YxL3BraS9jcmwwDQYJKoZIhvcN
AQELBQADggEBAEAgIWeYvHgXir6jzmC0TIqowCKUIKZ90LWE1g7P5Hi9y7mkpJvA
MB+NSFL4nYdrEgL7KIgQNqqS9hWakpc38ALTN8L/eGa1+n+N0h8elIVuVTwnjf3p
EiuangEWLMCWlKgwHJgJZif9fobcmP7vBddNsKWYeAVeoEICZ2uMISHuVG1ZOmrU
fMqdVDNUD9Nm/n5lZDAP/yxLnkr2DihyW5UQWAF6SMgSvUODSzJy/okrWRF6cMCs
3/5oF601ZH46UAiWuxORKyXvO6EjeS5Hi0uc9pixspyIbMxVD6nlS7Ze7qTHqjbx
/86VSiz27XgIOYfUIN0/S4fYNf33DoRkbY0=
-----END CERTIFICATE-----
private_key         -----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEA3cC8LWJmm92SHSxh7hu8SOPu4Mu6d6sFWixshSOltXBuvugz
BSG4i0nrO6XJIDv6q7VRXOBoNvuVwLO1i5tPC3KJAj7Iw+dK0SqPLnOablrUwDsN
PtZHj5EYda6c98hkxWA8GNDHp4Mtk58aqR8+50Dmr7KRBLyULaPbgKsO0cMEnNqk
CyDHWMcr0knopZCI/TThji78mQaEB7tinxhzd6Wjx0HzPiN2Y+bsT6zVxyxESTz6
jvWMBCqVx8BgVtckOzxzVEBoSyDvg92uHZm8wCsOsdpeFhqbU2GvLopsV1QSA/g4
wI9VS59hD4GnrCCuZDRhUuZ+MCnbbuKHOmFRlQIDAQABAoIBAEX9iCdW8IXviCeX
E43AyUvETWg8RS1yGC1e6h2Xo7zBsOKmjTvoacPk388iw3leFP9PKlADMEFyZNC+
p+VZbrhxPRctU9apUO713N1PdYWxO4c03DhiD5IbvLmgFEEMyemWN0Gp2+peN+tp
A1Qv3X3F+UmpNaZmEurY1fYlh3bi3Pmm7dZc2gucYOeho8h985ipAHe9a2G/50Qh
bnLGXCtEA7tqrTIY03FYn5VnGP+sOYSye5QDXED9z9spzIFT9XzrkkwCoMmTOBMe
ixAdV53d5Mg52N26pHgYYtQ8vylqe9+c0RYtWVS9eFQXX47Z/Fw1CqRWgdtZZoKr
IlTxMMECgYEA/uUZuxiaosDZOekbM0Eh1I2ExVPHWmUEFFkedylaiMQP7U16KDtm
Fq/hQZDLV7JKTB7DG03q0BTr18yvHHait32ABamuPR1/4LeJPFCLF4PwW96R5L3p
o/5hxNz3ftcfNrTAAWL+lp9oPgHhWvgr+oMkNXxfFvWcTG9qj/hLH2UCgYEA3rbZ
7avijOTHqMSUXGGzWqPVX4hdBpich/auzvexh2zHXaWfvH8OvXmUz13BytQnH/GY
bOFWkliwVP6S+7vc3bMRLWb8ZTg/bUxKkO5hjm+VeCYPJY0RcRRDEa3q2ujs2U2m
00rSlFhk9v/qyI9ztClShX+PYigCppksCdJGPnECgYEA42K9cYqhaE9heafZ+/8+
jr8wklgKnzk+Smi2JNdfTGKbUrarIvjaOaLs7/CbdcA3R3Cp3NHFh5siSYDvNhUf
U1FBw8t7BEosqesRIh039+JbqZkDzWsd4o4r6dK1dxGxZrwYDSSiuPu7opVK1DxP
/0q+Iniw22p/5DAAgC6f1YECgYEAiazdQSgtT02qAzEqSYV3+wMmRv0kDIzQztf2
rii+TOo4wDI/caXVtdlv3VSnFLxbR0rxH/WYr7U1pAUPVaCHY2Frr/Zm9id0RhuQ
SNGj6wodiv10BZGUA6Qz5bzuXs74g0iWZS1uyZdvKqV/POY471lQEwiM2W/EW7p6
V8Pt+nECgYEA0URUwwXhKSh2r1cO3XAyUAQMPUqw4vyDAcrO4kgBcv1nLr2keTlt
pB57rMGi7/eJD0OhhLcvN1ZRr4WMR9J+I7pg0CJ6NftkXBfzgNZSZT4FE2aUTGAD
gUH1gVAWen9ggsu0S7w7co2YAg57ghacPgGKz635bAhAKyLkmC0HLtU=
-----END RSA PRIVATE KEY-----
private_key_type    rsa
serial_number       1f:4b:e7:c0:a6:0b:80:10:04:cf:a1:1c:8a:27:ff:f2:a6:d5:fe:a4 </details>

***
Добавляем наши сертификаты в доверенные системой

```
sudo cp CA_cert.crt /usr/local/share/ca-certificates/
sudo cp intermediate.cert.pem /usr/local/share/ca-certificates/
```

***
Начинаем работать с nginx
Устанавливаем nginx в систему
```
Sudo apt install nginx
```
Проверим работу сервера
 <summary>systemctl status nginx</summary><details>
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2022-01-29 00:03:08 MSK; 16min ago
       Docs: man:nginx(8)
    Process: 2746 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 2747 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 2836 (nginx)
      Tasks: 2 (limit: 1085)
     Memory: 4.0M
     CGroup: /system.slice/nginx.service
             ├─2836 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
             └─2839 nginx: worker process

янв 29 00:03:08 kurs systemd[1]: Starting A high performance web server and a reverse proxy server...
янв 29 00:03:08 kurs systemd[1]: Started A high performance web server and a reverse proxy server.
</details>

Проверим доступность сервера с физической машины. Ранее открывались доступы на ufw. 