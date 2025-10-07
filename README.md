# Домашнее задание к занятию «Docker. Часть 2» Муртазина И.А. группа fops-42

---

### Задание 1

**Напишите ответ в свободной форме, не больше одного абзаца текста.**

Установите Docker Compose и опишите, для чего он нужен и как может улучшить лично вашу жизнь.

### Решение 1

Docker Compose позволяет настраивать, запускать и управлять многоконтейнерными приложениями. С его помощью написав один конфигурационный файл можно описать всю инфраструктуру и развернуть многосервисное приложение. Так как вся конфигурация описана в одном файле, им можно делиться в организации для стандартизации окружения для всей команды и сэкономить время на его настройке.

---

### Задание 2 

**Выполните действия и приложите текст конфига на этом этапе.** 

Создайте файл docker-compose.yml и внесите туда первичные настройки: 

 * version;
 * services;
 * volumes;
 * networks.

При выполнении задания используйте подсеть 10.5.0.0/16.
Ваша подсеть должна называться: <ваши фамилия и инициалы>-my-netology-hw.
Все приложения из последующих заданий должны находиться в этой конфигурации.

### Решение 2

![Внесение первичных настроек](https://github.com/murtazinilyas/6.4_docker-compose_mia/blob/main/screenshots/d2t2.png)

С развитием Docker Compose атрибут **"version"** утратил свою значимость, так как теперь по умолчанию используется последняя спецификация Compose. Я его удалил, на функциональность файла Compose никак не повлияло в дальнейшем.

---

### Задание 3 

**Выполните действия:** 

1. Создайте конфигурацию docker-compose для Prometheus с именем контейнера <ваши фамилия и инициалы>-netology-prometheus. 
2. Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории [6-04/prometheus](https://github.com/netology-code/sdvps-homeworks/tree/main/lecture_demos/6-04/prometheus) ).
3. Обеспечьте внешний доступ к порту 9090 c докер-сервера.

### Решение 3

![Docker Compose с сервисом Prometheus](https://github.com/murtazinilyas/6.4_docker-compose_mia/blob/main/screenshots/d2t3ptsdc.png)
![Конфигурационный файл для Prometheus](https://github.com/murtazinilyas/6.4_docker-compose_mia/blob/main/screenshots/d2t3ptsconf.png)

В конфигурационном файле указаны интервалы сбора и подтверждения метрик. Также сразу добавил заранее сбор метрик из сервиса Pushgateway.

---

### Задание 4 

**Выполните действия:**

1. Создайте конфигурацию docker-compose для Pushgateway с именем контейнера <ваши фамилия и инициалы>-netology-pushgateway. 
2. Обеспечьте внешний доступ к порту 9091 c докер-сервера.

### Решение 4

![Добавление сервиса Pushgateway](https://github.com/murtazinilyas/6.4_docker-compose_mia/blob/main/screenshots/d2t4pgtwdc.png)

---

### Задание 5 

**Выполните действия:** 

1. Создайте конфигурацию docker-compose для Grafana с именем контейнера <ваши фамилия и инициалы>-netology-grafana. 
2. Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории [6-04/grafana](https://github.com/netology-code/sdvps-homeworks/blob/main/lecture_demos/6-04/grafana/custom.ini).
3. Добавьте переменную окружения с путем до файла с кастомными настройками (должен быть в томе), в самом файле пропишите логин=<ваши фамилия и инициалы> пароль=netology.
4. Обеспечьте внешний доступ к порту 3000 c порта 80 докер-сервера.

### Решение 5

![Добавление сервиса Grafana](https://github.com/murtazinilyas/6.4_docker-compose_mia/blob/main/screenshots/d2t5grdc.png)
![Конфигурационный файл для Grafana](https://github.com/murtazinilyas/6.4_docker-compose_mia/blob/main/screenshots/d2t5grconf.png)

---

### Задание 6 

**Выполните действия.**

1. Настройте поочередность запуска контейнеров.
2. Настройте режимы перезапуска для контейнеров.
3. Настройте использование контейнерами одной сети.
5. Запустите сценарий в detached режиме.

### Решение 6

1. Поочередность запуска контейнеров уже описана в параметре **depends_on** каждого сервиса - каждый следующий сервис зависит от предыдущего.
2. Режимы перезапуска контейнеров установлены как **unless_stopped**, это означает, что контейнер будет перезапущен автоматически, кроме случае, когда он был явно остановлен пользователем.
3. Все контейнеры запускаются в сети **murtazinia-my-netology-hw** подсеть **10.5.0.0/16**, основной шлюз **10.5.0.1**
4. Сценарий в фоновом режиме запускается командой `docker compose up -d`

---

### Задание 7 

**Выполните действия.**
1. Выполните запрос в Pushgateway для помещения метрики <ваши фамилия и инициалы> со значением 5 в Prometheus: ```echo "<ваши фамилия и инициалы> 5" | curl --data-binary @- http://localhost:9091/metrics/job/netology```.
2. Залогиньтесь в Grafana с помощью логина и пароля из предыдущего задания.
3. Cоздайте Data Source Prometheus (Home -> Connections -> Data sources -> Add data source -> Prometheus -> указать "Prometheus server URL = http://prometheus:9090" -> Save & Test).
4. Создайте график на основе добавленной в пункте 5 метрики (Build a dashboard -> Add visualization -> Prometheus -> Select metric -> Metric explorer -> <ваши фамилия и инициалы -> Apply.

В качестве решения приложите:

* docker-compose.yml **целиком**;
* скриншот команды docker ps после запуске docker-compose.yml;
* скриншот графика, постоенного на основе вашей метрики.

### Решение 7

[docker-compose.yaml](https://github.com/murtazinilyas/6.4_docker-compose_mia/blob/main/docker-compose.yaml)
```YAML
services:
  prometheus:
    image: prom/prometheus:main
    container_name: murtazinia-netology-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - 9090:9090
    volumes:
      - ./prometheus:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - murtazinia-my-netology-hw
    restart: unless-stopped

  pushgateway:
    image: prom/pushgateway:master
    container_name: murtazinia-netology-pushgateway
    ports:
      - 9091:9091
    networks:
      - murtazinia-my-netology-hw
    depends_on:
      - prometheus
    restart: unless-stopped

  grafana:
    image: grafana/grafana
    container_name: murtazinia-netology-grafana
    environment:
      GF_PATHS_CONFIG: /etc/grafana/custom.ini
    ports:
      - 80:3000
    volumes:
      - ./grafana:/etc/grafana
      - grafana-data:/var/lib/grafana
    networks:
      - murtazinia-my-netology-hw
    depends_on:
      - pushgateway
    restart: unless-stopped

volumes:
  prometheus-data:
  grafana-data:

networks:
  murtazinia-my-netology-hw:
    driver: bridge
    ipam:
      config:
      - subnet: 10.5.0.0/16
        gateway: 10.5.0.1
```
![Результат вывода команды docker ps](https://github.com/murtazinilyas/6.4_docker-compose_mia/blob/main/screenshots/d2t7ps.png)
![График на полученной метрике](https://github.com/murtazinilyas/6.4_docker-compose_mia/blob/main/screenshots/d2t7dsh.png)

---

### Задание 8

**Выполните действия:** 

1. Остановите и удалите все контейнеры одной командой.

В качестве решения приложите скриншот консоли с проделанными действиями.

### Решение 8

Остановка и удаление всех контейнеров, запущенных через Docker Compose, осуществляется командой `docker compose down` 

Остановка и удаление абсолютно всех контейнеров осуществляется командой `docker rm -f $(docker ps -aq)`

![Та-да!!!](https://github.com/murtazinilyas/6.4_docker-compose_mia/blob/main/screenshots/d2t8.png)

---

### Задание 9* 

**Выполните действия:** 

1. Создайте конфигурацию docker-compose для Alertmanager с именем контейнера <ваши фамилия и инициалы>-netology-alertmanager. 
2. Добавьте необходимые тома с данными и [конфигурацией](https://github.com/netology-code/sdvps-homeworks/tree/main/6-04/alertmanager), сеть, режим и очередность запуска.
3. Обновите конфигурацию Prometheus (необходимые изменения ищите в презентации или документации) и перезапустите его. 
4. Обеспечьте внешний доступ к порту 9093 c докер-сервера.

В качестве решения приложите скриншот с событием из Alertmanager.

### Решение 9

![ALARM!!!](https://github.com/murtazinilyas/6.4_docker-compose_mia/blob/main/screenshots/d2t9.png)

---

### Задание 10* 

Запустите свой сценарий на чистом железе без предзагруженных образов.

**Ответьте на вопросы в свободной форме:**

1. Опишите выполненный вами процесс развертывания сценария.
2. Как вы думаете зачем может понадобиться такой способ развертывания?

### Решение 10

Запуск сценария, описанного в Docker Compose, без предзагруженных образов будет идти дольше из-за загрузки указанных образов с Docker Hub'а. Может понадобиться регистрация машины на Docker Hub. По-крайней мере, ничего другого я не заметил.
