# Домашнее задание к занятию "`ELK`" - `Огай Андрей`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. В личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

## Задание 1. 
Была выполнена установка пакетов Elasticsearch и Kibana из официального репозитория Elastic, их запуск и добавление в автозагрузку systemd.

Команды для установки и запуска:

1. wget -qO - [https://artifacts.elastic.co/GPG-KEY-elasticsearch](https://artifacts.elastic.co/GPG-KEY-elasticsearch) | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
2. echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] [https://artifacts.elastic.co/packages/7.x/apt](https://artifacts.elastic.co/packages/7.x/apt) stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
3. sudo apt update
sudo apt install elasticsearch kibana -y
4. sudo systemctl daemon-reload
5. sudo systemctl enable --now elasticsearch kibana

## Задание 2 
Был остановлен сторонний сервис (HAProxy), занимавший порт 80, после чего установлен и запущен веб-сервер Nginx.

Команды:
1. sudo systemctl stop haproxy
2. sudo systemctl disable haproxy
3. sudo apt install nginx -y
4. sudo systemctl enable --now nginx

Проверка работы веб-сервера:
1. curl http://localhost

## Задание 3 
Установлен Logstash и создан конфигурационный файл /etc/logstash/conf.d/nginx.conf для сбора файла логов /var/log/nginx/access.log и отправки их в Elasticsearch.

Конфигурация /etc/logstash/conf.d/nginx.conf:

input {
  file {
    path => "/var/log/nginx/access.log"
    start_position => "beginning"
    sincedb_path => "/dev/null"
  }
}

output {
  elasticsearch {
    hosts => ["http://localhost:9200"]
    index => "nginx-logstash-%{+YYYY.MM.dd}"
  }
}

В Kibana был создан Index Pattern nginx-logstash-*, в разделе Discover подтверждено поступление логов Nginx через Logstash.

## Задание 4 
Установлен агент Filebeat, включен модуль nginx, а в конфигурации /etc/filebeat/filebeat.yml указан вывод в Elasticsearch (localhost:9200). Выполнена команда sudo filebeat setup для автоматической загрузки шаблонов и дашбордов в Kibana.

Команды настройки:
1. sudo apt install filebeat -y
2. sudo filebeat modules enable nginx
3. sudo filebeat setup
4. sudo systemctl enable --now filebeat

В Kibana в разделе Discover выбран автоматически созданный паттерн filebeat-* и подтверждено отображение событий event.module: nginx.

ВСЕ СКРИНШОТЫ С РЕШЕНИЯМИ ПРЕДОСТАВЛЕНЫ В ПАПКЕ img 
