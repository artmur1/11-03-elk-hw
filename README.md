# Домашнее задание к занятию "`ELK`" - `Мурчин Артем`

## Дополнительные ресурсы

При выполнении задания используйте дополнительные ресурсы:
- [docker-compose elasticsearch + kibana](11-03/docker-compose.yaml);
- [поднимаем elk в docker](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/docker.html);
- [поднимаем elk в docker с filebeat и docker-логами](https://www.sarulabs.com/post/5/2019-08-12/sending-docker-logs-to-elasticsearch-and-kibana-with-filebeat.html);
- [конфигурируем logstash](https://www.elastic.co/guide/en/logstash/7.17/configuration.html);
- [плагины filter для logstash](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html);
- [конфигурируем filebeat](https://www.elastic.co/guide/en/beats/libbeat/5.3/config-file-format.html);
- [привязываем индексы из elastic в kibana](https://www.elastic.co/guide/en/kibana/7.17/index-patterns.html);
- [как просматривать логи в kibana](https://www.elastic.co/guide/en/kibana/current/discover.html);
- [решение ошибки increase vm.max_map_count elasticsearch](https://stackoverflow.com/questions/42889241/how-to-increase-vm-max-map-count).

**Примечание**: если у вас недоступны официальные образы, можете найти альтернативные варианты в DockerHub, например, [такой](https://hub.docker.com/layers/bitnami/elasticsearch/7.17.13/images/sha256-8084adf6fa1cf24368337d7f62292081db721f4f05dcb01561a7c7e66806cc41?context=explore).

### Задание 1. Elasticsearch

Установите и запустите Elasticsearch, после чего поменяйте параметр cluster_name на случайный. 

*Приведите скриншот команды 'curl -X GET 'localhost:9200/_cluster/health?pretty', сделанной на сервере с установленным Elasticsearch. Где будет виден нестандартный cluster_name*.

### Решение 1. Elasticsearch

![alt text](https://github.com/artmur1/11-03-elk-hw/blob/main/elk-mas-zad1.png)

---

### Задание 2. Kibana

Установите и запустите Kibana.

*Приведите скриншот интерфейса Kibana на странице http://<ip вашего сервера>:5601/app/dev_tools#/console, где будет выполнен запрос GET /_cluster/health?pretty*.

### Решение 2. Kibana

![alt text](https://github.com/artmur1/11-03-elk-hw/blob/main/elk-mas-zad2.png)

---

### Задание 3. Logstash

Установите и запустите Logstash и Nginx. С помощью Logstash отправьте access-лог Nginx в Elasticsearch. 

*Приведите скриншот интерфейса Kibana, на котором видны логи Nginx.*

### Решение 3. Logstash

![alt text](https://github.com/artmur1/11-03-elk-hw/blob/main/elk-mas-zad3-1.png)

В каталоге /etc/logstash/conf.d/ создал файл access.conf с настройкой поставки access-лога nginx в elasticsearch.

![alt text](https://github.com/artmur1/11-03-elk-hw/blob/main/elk-mas-zad3-2.png)

В пайплайне укзан путь к каталогу с конфигурационным файлом, который был создан.

![alt text](https://github.com/artmur1/11-03-elk-hw/blob/main/elk-mas-zad3-3.png)

Тут видно, что логи nginx записываются в файл.

---

### Задание 4. Filebeat. 

Установите и запустите Filebeat. Переключите поставку логов Nginx с Logstash на Filebeat. 

*Приведите скриншот интерфейса Kibana, на котором видны логи Nginx, которые были отправлены через Filebeat.*

### Решение 4. Filebeat. 

![alt text](https://github.com/artmur1/11-03-elk-hw/blob/main/elk-mas-zad4-1.png)

Конфиг файлбит

![alt text](https://github.com/artmur1/11-03-elk-hw/blob/main/elk-mas-zad4-2.png)

В каталоге /etc/logstash/conf.d/ изменил файл access.conf

![alt text](https://github.com/artmur1/11-03-elk-hw/blob/main/elk-mas-zad4-3.png)

Видно процесс filebeat в elasticsearch

![alt text](https://github.com/artmur1/11-03-elk-hw/blob/main/elk-mas-zad4-4.png)

Но логи в кибану не идут..

### Доработка

![alt text](https://github.com/artmur1/11-03-elk-hw/blob/main/elk-mas-zad4-5.png)

Тут видно, что nginx логи формирует нормально.

- Установка и настройка vector

![alt text](https://github.com/artmur1/11-03-elk-hw/blob/main/zad4_dorabotka_1.png)

Установил vector, добавил в elscticsearch

![alt text](https://github.com/artmur1/11-03-elk-hw/blob/main/zad4_dorabotka_2.png)

Конфигурация файла vector.yml

![alt text](https://github.com/artmur1/11-03-elk-hw/blob/main/zad4_dorabotka_4.png)

Ура, через vector логи в elscticsearch идут!

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 5*. Доставка данных 

Настройте поставку лога в Elasticsearch через Logstash и Filebeat любого другого сервиса , но не Nginx. 
Для этого лог должен писаться на файловую систему, Logstash должен корректно его распарсить и разложить на поля. 

*Приведите скриншот интерфейса Kibana, на котором будет виден этот лог и напишите лог какого приложения отправляется.*
