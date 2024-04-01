# Мониторинг
Проект создаётся как комплексное решение по мониторингу экосистемы (серверов, контейнеров, веб-приложений). 
## Состав
В его состав будут входить:
* Portainer - WebUI для отображения работающих контейнеров, их рестарта
* Nginx для единой точки входа
* Prometheus для сбора данных о CPU c Linux/Windows серверов
* alertmanager для уведомлений в Telegram ботом в группу
* Zabbix для опроса серверов и рестарт/переключения на горячий резерв
* MySQL - в качестве хранилища для Zabbix
* ElasticSearch - для хранения логов
* XWiki - документация
## Технология
В качестве объединяющей технологии выбран docker-compose. Это позволит склеить docker-compose конфиги различных проектов, а интерфейс - получать статус по всем сервисам.
В основе лежит скрипт, который скачивает репозитории проектов с GitHub и запускает в них docker-compose конфигурации.
## Ссылки на проекты
* https://hub.docker.com/extensions/portainer/portainer-docker-extension :2.19.4
* https://hub.docker.com/_/nginx :1:25
* https://github.com/docker/awesome-compose/blob/master/prometheus-grafana/compose.yaml
* https://github.com/zabbix/zabbix-docker/blob/6.4/docker-compose_v3_alpine_mysql_latest.yaml
* https://github.com/docker/awesome-compose/blob/master/elasticsearch-logstash-kibana/compose.yaml
* https://github.com/xwiki/xwiki-docker?tab=readme-ov-file#using-docker-compose
## Правила
* Каждое прилоежение с своём контейнере
* Используем, при наличии, официальные docker-compose
* Используем хелсчеки для проверки готовности
* На образы указываем версию во избежании несовместимости
## Состав
* start.sh скачивает docker-compose конфиги и запускает их
* docker-compose.portainer.yaml
* docker-compose.nginx.yaml
* nginx.conf для URL:
```
server {
    listen 80;
    server_name elastic.$host;
    location / {
        proxy_pass http://localhost:9200;
    }
}
```
