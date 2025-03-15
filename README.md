Источник: [https://github.com/netology-code/loadqa-homeworks](https://github.com/netology-code/loadqa-homeworks)

#### ---Команды docker
`docker compose up -d` - запуск файла настройки docker-compose.yml из текущей папки\
`docker compose down` - остановка и удаление контейнеров, запущенных командой указанной выше\
`docker ps` - состояние докера\
`docker restart telegraf` - перезапуск контейнера telegraf\
***


# Домашнее задание к лекции 4 «Проведение нагрузочного тестирования DB»

Любые вопросы по решению задач задавайте в чате учебной группы.

### Задание 1

Провести тестирование процедур dorepeat_v1 и dorepeat_v2, прислать скриншоты графиков.

#### Инструкция:

- склонировать репозиторий с сервисом wordpress
    ```bash
    git clone https://github.com/mshegolev/congenial-potato.git

    cd congenial-potato/wp
    ```

- в файле docker-compose.yml поменять версию образа wordpress на wordpress:php8.0
  ```yaml
    wordpress:
    depends_on:
      - database
    image: wordpress:php8.0
    restart: always
    ports:
      - "80:80"
    environment:
      WORDPRESS_DB_HOST: database:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppassword
      WORDPRESS_DB_NAME: wpdb
    volumes:
      ["./:/var/www/html"]
  ```

- Запустить создание докер-контейнеров
  ```bash
  docker compose up -d 
  ```

- Добавить в папку 
  ```bash
  apache-jmeter-{version}/lib 
  ``` 
  файл mysql-connector-j-8.4.0.jar

#### Что нужно сделать:

Нужно провести тестирование хранимой процедуры:
1. Измерить время отклика вызова процедуры dbrepeat_v1.
2. Измерить время отклика вызова процедуры dbrepeat_v2. Для процедуры нужно дополнительно передать текст комментария p2.
    ```log
    График в JMeter для оценки времени отклика вот тут:
    Add -> Listener -> jp@gc - Response Times Over Time
    ```
    ```log
    Чтобы дополнительные графики появились, нужно скачать через Plugins Manager:
    - 3 Basic Graphs
    - 5 Additional Graphs
    ```

<details>
  <summary>Если во время теста появится ошибка:</summary>

    По умолчанию для работы JMeter компьютер выделяет 1 GB памяти, чтобы обеспечить стабильную работу тестов на машине. Однако, для ресурсоемких тестов, этого объема может не хватать, что приводит к ошибкам из-за нехватки памяти.
    Решением в этой ситуации является увеличение выделенной для JMeter памяти:

    Откройте файл jmeter.bat, если работаете в Windows, или jmeter.sh, если работаете на Linux или на Mac, в текстовом редакторе.
    Найдите строчку:

        set HEAP=-Xms1g -Xmx1g

    Увеличьте память до комфортного вам объема. Учтите, что JMeter не должен использовать всю память вашего компьютера, поэтому не выделяйте больше памяти, чем у вас доступно. Рекомендуется установить 2 GB, этого должно быть достаточно для ваших задач:

        set HEAP=-Xms2g -Xmx2g
</details>

#### Отправка задания на проверку

Нужно запушить репозиторий с конфигурацией, дашбордами и скриншотами на GitHub и отправить на проверку ссылку на репозиторий.

