## Задача 1
- Опишите своими словами основные преимущества применения на практике IaaC паттернов.  
 - Неограниченое маштабирование инфраструктуры (кроме финансовой)
 - Ускорение производства выпуска продукта
 - Стабильность и отсутствие дрейфа конфигурации 
 - Быстрая и эффективная разработка
- Какой из принципов IaaC является основополагающим?  
    - CI/CD - непрерывная интеграция/непрерывная доставка


## Задача 2
- Чем Ansible выгодно отличается от других систем управление конфигурациями?
  - Ansible позволяет управлять всеми известными системами благодаря доп. модулям. Так же большой плюс что не нужна установка ПО на клиентские машины

- Какой, на ваш взгляд, метод работы систем конфигурации более надёжный push или pull?
 - `push` - потому что сервер инициирует отправку конфигурации, а не клиентская машина.


## Задача 3
- vagrant box list
    bento/ubuntu-20.04 (virtualbox, 202107.28.0)

-  vagrant -v
    Vagrant 2.2.19 

- ansible —version 
    ansible 2.9.6



## Задача 4

```
 vagrant@vagrant:~$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
OK
vagrant@vagrant:~$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
vagrant@vagrant:~$ sudo apt update
vagrant@vagrant:~$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
vagrant@vagrant:~$ sudo usermod -aG docker ${USER}
vagrant@vagrant:~$ su - ${USER}
vagrant@vagrant:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
vagrant@vagrant:~$ docker -v
Docker version 20.10.16, build aa7e414
```
