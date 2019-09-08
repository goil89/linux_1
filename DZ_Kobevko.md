# Домашнее задание (досдача)

## Настроить сетевой фильтр, чтобы был доступ только к сервисам http и ssh.

### Сначала я запрещаю все входящие, исходящие и проходящие пакеты
### Далее разрешаю входящие и исходящие пакеты в локальной петле
### Потом разрешаю все ИСХОДЯЩИЕ соединения по всем протоколам и портам
### Разрешаю TCP- и UDP-пакеты, которые были запрошены локальными приложениями
### Далее устанавливаю правила для SSH и HTTP
```bash
sudo iptables -P INPUT DROP
sudo iptables -P OUTPUT DROP
sudo iptables -P FORWARD DROP

sudo iptables -A INPUT -i lo -j ACCEPT 
sudo iptables -A OUTPUT -o lo -j ACCEPT

sudo iptables -P OUTPUT ACCEPT
 
sudo iptables -A INPUT -p TCP -m state --state ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A INPUT -p UDP -m state --state ESTABLISHED,RELATED -j ACCEPT

sudo iptables -A INPUT -p tcp -m tcp --dport 22 -j ACCEPT
sudo iptables -A OUTPUT -p tcp -m tcp --sport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
sudo iptables -A OUTPUT -p tcp -m tcp --sport 80 -j ACCEPT
```