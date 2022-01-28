UFW уже предустановлен, однако sudo apt install ufw для установки и sudo ufw enable для включения



ssh 192.168.56.1 -l hello 
# где -l ключ для логина

Открытие портов 
sudo ufw allow 443/tcp
sudo ufw allow 80/tcp
sudo ufw allow 22/tcp


установка vault
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -

sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"

$ sudo apt-get update && sudo apt-get install vault

Запускаем сервер vault
vault server -dev -dev-root-token-id root
Открываем новое окно терминала и в нём работаем с сервером vault. В нём задаём переменные VAULT_ADDR и VAULT_TOKEN

```

export VAULT_ADDR=http://127.0.0.1:8200
export VAULT_TOKEN=root

```

***
Приступаем к генерации сертификата
Включаем PKI
vault secrets enable pki
>>>Success! Enabled the pki secrets engine at: pki/
Задаём TTL
vault secrets tune -max-lease-ttl=87600h pki
Генерируем корневой сертификат
vault write -field=certificate pki/root/generate/internal \
     common_name="example.com" \
     ttl=87600h > CA_cert.crt

Прописываем ссылки для сертификата
$ vault write pki/config/urls \
>      issuing_certificates="$VAULT_ADDR/v1/pki/ca" \
>      crl_distribution_points="$VAULT_ADDR/v1/pki/crl"
>>>Success! Data written to: pki/config/urls