#Docker 


### 1. Создание Spring Boot приложения

**1.1. Инициализация проекта:**

- Создания Spring Boot приложения.

**1.2. Настройка конфигурации:**

- В файле `application.properties` (или `application.yml`) настройте параметры подключения к базе данных:

```properties
spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}

spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=update
```

Использование переменных окружения позволяет гибко изменять настройки в разных средах.

**1.3. Сборка maven:**

- Сделать `package` и в результате получить `.jar` файл. 

### 2. Создание Dockerfile

**2.1. Создание файла:**

- В корне вашего проекта создайте файл `Dockerfile` со следующим содержимым:

```Dockerfile
FROM openjdk:21-jdk-slim

WORKDIR /app

COPY target/*.jar app.jar

ENTRYPOINT ["java","-jar","/app/app.jar"]
```

- Этот файл описывает процесс сборки Docker образа вашего приложения.

### 3. Сборка Docker образа

- Откройте терминал в директории вашего проекта и выполните команду:

```bash
docker build -t your-image-name .
```

Замените `your-image-name` на желаемое имя вашего образа.

### 4. Создание файла docker-compose.yml

- Создайте файл `docker-compose.yml` со следующим содержимым:

    ```yml
    version: '3.7'
    
    services:
      app:
        build: .
        container_name: your_app_name
        ports:
          - "8080:8080"
        environment:
          DB_URL: jdbc:postgresql://db:5432/postgres
          DB_USERNAME: postgres
          DB_PASSWORD: postgres
        depends_on:
          - db
    
      db:
        image: postgres
        container_name: your_db_name
        environment:
          POSTGRES_USER: postgres
          POSTGRES_PASSWORD: postgres
        volumes:
          - pgdata:/var/lib/postgresql/data
    
    volumes:
      pgdata:
    ```

- Этот файл описывает конфигурацию много контейнерного приложения.

### 5. Запуск контейнеров

- Выполните команду:

```bash
docker-compose up -d
```

- Это запустит контейнеры с вашим приложением и базой данных в фоновом режиме.

### Пояснения к файлу docker-compose.yml

- **version:** Указывает версию формата Docker Compose.
- **services:** Определяет сервисы (контейнеры), которые будут запущены.
- **app:**
    
    - **build:** Указывает, что образ для этого сервиса нужно собрать из текущей директории.
    - **container_name:** Имя контейнера.
    - **ports:** Маппинг портов: локальный порт 8080 будет перенаправлен на порт 8080 внутри контейнера.
    - **environment:** Переменные окружения, которые будут доступны внутри контейнера.
    - **depends_on:** Указывает, что сервис `app` зависит от сервиса `db`.
    
- **db:**
    
    - **image:** Имя образа Docker для базы данных PostgreSQL.
    - **container_name:** Имя контейнера с базой данных.
    - **environment:** Переменные окружения для настройки базы данных.
    - **volumes:** Создает том `pgdata` и монтирует его в директорию `/var/lib/postgresql/data` внутри контейнера.
    

### Дополнительные замечания

- **Том `pgdata`:** Используется для сохранения данных базы данных. При удалении контейнера данные будут сохранены.