### Задание 1

**Напишите ответ в свободной форме, не больше одного абзаца текста.**

Установите Docker Compose и опишите, для чего он нужен и как может улучшить вашу жизнь.
#### Ответ
![image](https://github.com/Flirex1/docker2/assets/133591860/84866966-fc89-49d2-bbd0-16141fa7f9bf)
Docker Compose используется для одновременного управления несколькими контейнерами, входящими в состав приложения. Этот инструмент предлагает те же возможности, что и Docker, но позволяет работать с более сложными приложениями.
---

### Задание 2 

**Выполните действия и приложите текст конфига на этом этапе.** 

Создайте файл docker-compose.yml и внесите туда первичные настройки: 

 * version;
 * services;
 * networks.

При выполнении задания используйте подсеть 172.22.0.0.
Ваша подсеть должна называться: <ваши фамилия и инициалы>-my-netology-hw.

---

### Задание 3 

**Выполните действия и приложите текст конфига текущего сервиса:** 

1. Установите PostgreSQL с именем контейнера <ваши фамилия и инициалы>-netology-db. 
2. Предсоздайте БД <ваши фамилия и инициалы>-db.
3. Задайте пароль пользователя postgres, как <ваши фамилия и инициалы>12!3!!
4. Пример названия контейнера: ivanovii-netology-db.
5. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

---

### Задание 4 

**Выполните действия:**

1. Установите pgAdmin с именем контейнера <ваши фамилия и инициалы>-pgadmin. 
2. Задайте логин администратора pgAdmin <ваши фамилия и инициалы>@ilove-netology.com и пароль на выбор.
3. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.
4. Прокиньте на 80 порт контейнера порт 61231.

В качестве решения приложите:

* текст конфига текущего сервиса;
* скриншот админки pgAdmin.
#### Ответ
Скриншот из админки ![image](https://github.com/Flirex1/docker2/assets/133591860/79ea173b-14aa-4f9e-aea7-43bc1b0f3ab6)
Часть конфига
version: "3"
services:

  netology-db:
    image: postgres:latest
    container_name: Perfilov-netology-db
    restart: always
    ports:
      - "5432:5432"
    volumes:
      - ./pg_data:/var/lib/postgresql/data/pgdata
    environment:
      POSTGRES_PASSWORD: PerfilovDI12!3!!
      POSTGRES_USER: postgres
      POSTGRES_DB: PerfilovDI-netology_db
      POSTGRES_INITDB_ARGS: --auth-host=scram-sha-256
      PGDATA: /var/lib/postgresql/data/pgdata 
    networks:
      PerfilovDI-my-netology-hw:
        ipv4_address: 172.22.0.2

  pgadmin:
    image: dpage/pgadmin4
    container_name: PerfilovDI-pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: PerfilovDI@ilove-netology.com
      PGADMIN_DEFAULT_PASSWORD: 123
    ports:
      - "61231:80"
    networks:
      PerfilovDI-my-netology-hw:
        ipv4_address: 172.22.0.3
    restart: always
    networks:
  PerfilovDI-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 172.22.0.0/24
---

### Задание 5 

**Выполните действия и приложите текст конфига текущего сервиса:** 

1. Установите Zabbix Server с именем контейнера <ваши фамилия и инициалы>-zabbix-netology. 
2. Настройте его подключение к вашему СУБД.
3. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

---

### Задание 6

**Выполните действия и приложите текст конфига текущего сервиса:** 

1. Установите Zabbix Frontend с именем контейнера <ваши фамилия и инициалы>-netology-zabbix-frontend. 
2. Настройте его подключение к вашему СУБД.
3. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

---

### Задание 7 

**Выполните действия.**

Настройте линки, чтобы контейнеры запускались только в момент, когда запущены контейнеры, от которых они зависят.

В качестве решения приложите:

* текст конфига **целиком**;
* скриншот команды docker ps;
* скриншот авторизации в админке Zabbix.
#### Ответ
Скриншот из админки Zabbix 
![image](https://github.com/Flirex1/docker2/assets/133591860/9cc9bd5e-036d-4974-b898-878ffcc37ce4)
Полный текст конфига: 
version: "3"
services:

  netology-db:
    image: postgres:latest
    container_name: Perfilov-netology-db
    restart: always
    ports:
      - "5432:5432"
    volumes:
      - ./pg_data:/var/lib/postgresql/data/pgdata
    environment:
      POSTGRES_PASSWORD: PerfilovDI12!3!!
      POSTGRES_USER: postgres
      POSTGRES_DB: PerfilovDI-netology_db
      POSTGRES_INITDB_ARGS: --auth-host=scram-sha-256
      PGDATA: /var/lib/postgresql/data/pgdata 
    networks:
      PerfilovDI-my-netology-hw:
        ipv4_address: 172.22.0.2

  pgadmin:
    image: dpage/pgadmin4
    container_name: PerfilovDI-pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: PerfilovDI@ilove-netology.com
      PGADMIN_DEFAULT_PASSWORD: 123
    ports:
      - "61231:80"
    networks:
      PerfilovDI-my-netology-hw:
        ipv4_address: 172.22.0.3
    restart: always

  zabbix-server:
    image: zabbix/zabbix-server-pgsql
    links:
      - netology-db
    container_name: PerfilovDI-netology-zabbix
    environment:
DB_SERVER_HOST: '172.22.0.2'
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: PerfilovDI12!3!!
    ports:
      - "10051:10051"
    networks:
      PerfilovDI-my-netology-hw:
        ipv4_address: 172.22.0.4
    restart: always
    depends_on:
      - netology-db


  zabbix-web:
    image: zabbix/zabbix-web-nginx-pgsql
    container_name: zabbix-web
    hostname: zabbix-web
    restart: always
    environment:
      DB_SERVER_HOST: netology-db
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: PerfilovDI12!3!!
      ZBX_SERVER_HOST: zabbix-server
      PHP_TZ: "Europe/Moscow"
    ports:
      - 80:8080
      - 443:8443
    networks:
      PerfilovDI-my-netology-hw:
        ipv4_address: 172.22.0.5
    depends_on:
      - netology-db
      - zabbix-server

networks:
  PerfilovDI-my-netology-hw:
 driver: bridge
    ipam:
      config:
        - subnet: 172.22.0.0/24
 Скрин docker ps
 ![image](https://github.com/Flirex1/docker2/assets/133591860/acb9041c-94ee-4095-8ea3-3ee2d3c9ab17)

---

### Задание 8 

**Выполните действия:** 

1. Убейте все контейнеры и потом удалите их.
1. Приложите скриншот консоли с проделанными действиями.
#### Ответ
![image](https://github.com/Flirex1/docker2/assets/133591860/db7eb958-2441-4a4f-9f01-01573b5c34a9)

---
