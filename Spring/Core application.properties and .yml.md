



Файл `application.properties` — это **основной конфигурационный файл** в Spring Boot, где вы задаете настройки приложения. Он работает по принципу **«конфигурация через свойства»** и позволяет гибко управлять поведением приложения без изменения кода. Вот как это работает:

---

### **1. Где находится и как подключается?**

- **Расположение**: Файл должен находиться в папке `src/main/resources/application.properties`.  
- **Автозагрузка**: Spring Boot автоматически обнаруживает и загружает его при старте приложения.

---

### **2. Синтаксис**

- Каждая строка — это **пара "ключ=значение"**.
- Ключи используют **точечную нотацию** для иерархии.  

Пример:
```properties
server.port=8080
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
logging.level.org.springframework=DEBUG
```

---

### **3. Что можно настроить?**

#### **Основные настройки**

- **Порт сервера**:
```properties
  server.port=9090
```

- **Контекстный путь (base URL)**:
```properties
  server.servlet.context-path=/api
```

- **Настройки базы данных**:
```properties
  spring.datasource.url=jdbc:mysql://localhost:3306/mydb
  spring.datasource.username=admin
  spring.datasource.password=secret
  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```
- **JPA/Hibernate**:
  ```properties
  spring.jpa.show-sql=true
  spring.jpa.hibernate.ddl-auto=update
  spring.jpa.properties.hibernate.format_sql=true
  ```

#### **Логирование**
```properties
logging.level.root=INFO
logging.level.org.springframework.web=DEBUG
logging.file.name=app.log
```

#### **Безопасность (Spring Security)**
```properties
spring.security.user.name=user
spring.security.user.password=password
```

#### **Другие модули**
- **Кэширование**:
  ```properties
  spring.cache.type=redis
  ```
- **Эндпоинты Actuator**:
  ```properties
  management.endpoints.web.exposure.include=health,info,metrics
  ```

---

### **4. Профили (Profiles)**
Чтобы управлять настройками для разных окружений (dev, prod, test), используйте **профили**:
- Создайте файлы с именем `application-{profile}.properties`.  
  Например:  
  - `application-dev.properties` — для разработки,  
  - `application-prod.properties` — для продакшена.
  
- Активируйте профиль:
  ```properties
  # В основном application.properties
  spring.profiles.active=dev
  ```
  Или при запуске:
  ```bash
  java -jar app.jar --spring.profiles.active=prod
  ```

---

### **5. Как использовать свойства в коде?**
#### **Через аннотацию `@Value`**
```java
import org.springframework.beans.factory.annotation.Value;

@Component
public class MyComponent {
    @Value("${server.port}")
    private int serverPort;

    @Value("${app.message:Hello World}") // значение по умолчанию
    private String message;
}
```

#### **Через классы-конфигурации (`@ConfigurationProperties`)**
1. Создайте класс:
```java
@Configuration
@ConfigurationProperties(prefix = "app")
public class AppConfig {
    private String name;
    private int version;
    private List<String> servers;

    // Геттеры и сеттеры обязательны!
}
```
2. Задайте свойства в `application.properties`:
```properties
app.name=MySuperApp
app.version=2
app.servers=server1,server2,server3
```
3. Внедрите конфиг в другие компоненты:
```java
@Service
public class MyService {
    private final AppConfig appConfig;

    public MyService(AppConfig appConfig) {
        this.appConfig = appConfig;
    }
}
```

---

### **6. Переопределение свойств**
Spring Boot позволяет **переопределять свойства** из `application.properties`:
- **Через аргументы командной строки**:
  ```bash
  java -jar app.jar --server.port=7070
  ```
- **Через переменные окружения**:
  ```bash
  export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost/mydb
  ```

---

### **7. Формат YAML**
Если вам не нравится синтаксис `.properties`, можно использовать **YAML** (файл `application.yml`):
```yaml
server:
  port: 8080
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydb
    username: admin
    password: secret
```

---

### **8. Важные нюансы**
- **Приоритеты**:  
  Свойства из командной строки > переменные окружения > `application.properties` > значения по умолчанию.
- **Безопасность**:  
  **Не храните пароли в `application.properties`**! Используйте шифрование или внешние хранилища (Vault, Kubernetes Secrets).
- **Кастомные свойства**:  
  Вы можете добавлять свои ключи (например, `app.api.key=12345`) и использовать их через `@Value`.

---

### **Пример полного файла**
```properties
# Server
server.port=8080
server.servlet.context-path=/api

# Database
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

# Logging
logging.level.root=INFO
logging.level.org.springframework.web=DEBUG

# Custom properties
app.name=MySpringApp
app.environment=dev
```

---

Если вы все настроите правильно, Spring Boot автоматически применит указанные параметры, и ваше приложение будет работать именно так, как вы задумали! 🚀