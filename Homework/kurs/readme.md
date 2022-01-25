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

