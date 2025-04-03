# Настройка балансировки Web с помощью HaRoxy

1. Установим HaProxy 
```bash
apt install haproxy
yum install haproxy
```
2. Скопируем оригинальный файл конфигурции 
```bash
cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.orig
```
3. Примерная конфигурация для балансировки 2-ух серверов:

```bash
global
    log 127.0.0.1 local0
    maxconn 4096

defaults
    log global
    #option httplog
    timeout connect 5s
    timeout client  50s
    timeout server  50s

frontend http_front
    bind *:80
    default_backend web_servers

backend web_servers
    balance roundrobin
    server server1 192.168.90.69:80 check weight 3
    server server2 192.168.90.68:80 check weight 1
```
4. Балансировка в данном случае Round-Robin - то есть, балансировка по очереди.

5. Далее все просто: направляем домен на IP адрес с HaProxy и он сам будет баланировать запросы.


