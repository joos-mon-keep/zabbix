# zabbix
Zabbix

Zabbix Ansible

https://github.com/ansible-collections/community.zabbix/tree/main/roles

**Установка Zabbix server на Debian 11**

С помощью инструкции с официального сайта установите Zabbix server  на операционную систему:

Установите PostgreSQL:
```
sudo apt install postgresql
```
Добавьте репозиторий Zabbix:
```
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4%2Bdebian11_all.deb 
dpkg -i zabbix-release_6.0-4+debian11_all.deb
apt update
```
Запустите Zabbix server:
```
sudo apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts nano -y # zabbix-agent
```
Создайте пользователя БД:
```
sudo -u postgres createuser --pwprompt zabbix
```
Создайте БД:
```
sudo -u postgres createdb -O zabbix zabbi
```
При автоматизации с помощью bash можно использовать следующие примеры:

Создание пользователя с помощью psql из-под root
```
su - postgres -c 'psql --command "CREATE USER zabbix WITH PASSWORD '\'123456789\'';"'
su - postgres -c 'psql --command "CREATE DATABASE zabbix OWNER zabbix;"
```
Импортируем схему:
```
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```
Задаём пароль в DBPassword:
```
sudo nano /etc/zabbix/zabbix_server.conf
```
Запускаем Zabbix server, Zabbix agent и веб-сервер:
```
sudo systemctl restart zabbix-server apache2 # zabbix-agent 
sudo systemctl enable zabbix-server apache2 # zabbix-agent
```
При автоматизации с помощью bash можно использовать следующий пример:

Настраиваем пароль DBPassword в файле /etc/zabbix/zabbix_server.conf:
```
sed -i 's/# DBPassword=/DBPassword=123456789/g' /etc/zabbix/zabbix_server.conf
```
Установка Zabbix agent похожа на установку Zabbix server:

Добавьте репозиторий Zabbix:
```
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4%2Bdebian11_all.deb
dpkg -i zabbix-release_6.0-4+debian11_all.deb 
apt update
```
Установите Zabbix agent и компоненты:
```
sudo apt install zabbix-agent -y
```
Запустите Zabbix agent:
```
sudo systemctl restart zabbix-agent 
sudo systemctl enable zabbix-agent
```
- zabbix_agentd.conf - Файл конфигурации Zabbix agent
- zabbix_agentd.log - Файл логов Zabbix agent

Меняем адрес сервера в zabbix_agentd.conf
```
sed -i 's/Server=127.0.0.1/Server=192.168.0.138'/g' /etc/zabbix/zabbix_server.conf
```
