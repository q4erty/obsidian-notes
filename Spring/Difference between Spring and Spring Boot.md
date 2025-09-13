#theory #springboot



Spring Framework и Spring Boot тесно связаны, но решают разные задачи. Вот ключевые отличия:

---

### ==**1. Основная цель**==
#### **Spring Framework:**  

  Это базовая платформа для создания Java-приложений.  
  Предоставляет модули для DI (Dependency Injection), MVC, Security, Data и т.д.,  
  но требует ручной настройки компонентов.


#### **Spring Boot:**  

  Надстройка над Spring Framework, которая упрощает создание приложений.  
  Основная цель — **минимизировать конфигурацию** и ускорить старт проекта.

---

### ==**2. Конфигурация**==

#### **Spring Framework:**

  Требует явной настройки:  
  - XML-файлы или Java-классы с аннотацией `@Configuration`.  
  - Ручное определение бинов, DataSource, сервлетов и т.д.  
  
>   Пример настройки DataSource в Spring:  
  ```java
  @Configuration
  public class AppConfig {
      @Bean
      public DataSource dataSource() {
          DriverManagerDataSource dataSource = new DriverManagerDataSource();
          dataSource.setUrl("jdbc:postgresql://localhost:5432/mydb");
          dataSource.setUsername("user");
          dataSource.setPassword("pass");
          return dataSource;
      }
  }
  ```

#### **Spring Boot:**  

  **Автоконфигурация**:  
  - Достаточно добавить зависимости (например, `spring-boot-starter-data-jpa`).  
  - Настройки указываются в `application.properties`:  

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
spring.datasource.username=user
spring.datasource.password=pass
```  

- Spring Boot сам создаст бины на основе этих параметров.

---

### ==**3. Встроенный сервер**==

#### **Spring Framework:**  

  Требует ручного развертывания на внешнем сервере (Tomcat, Jetty)  
  или интеграции через `web.xml`.


#### **Spring Boot:**  

  Поставляется со **встроенным сервером** (Tomcat по умолчанию).  

>   Запуск приложения — одной командой:  
  ```bash
  mvn spring-boot:run
  ```

---

### ==**4. Стартеры (Starters)**==
#### **Spring Framework:**  

  Нужно вручную добавлять зависимости в `pom.xml`/`build.gradle` и следить за их совместимостью.  
  
>   Например, для Spring MVC:  
  ```xml
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.20</version>
  </dependency>
  ```


#### **Spring Boot:**  

  Использует **стартеры** — предварительно собранные наборы зависимостей:  
  
  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  ```  
  
  Одна зависимость включает всё для веб-приложения (MVC, встроенный сервер, Jackson и т.д.).

---

### ==**5. Профили и настройки**==

#### **Spring Framework:**  

  Профили (`@Profile`) настраиваются вручную через XML или Java Config.


#### **Spring Boot:**  

  Упрощенная работа с профилями через файлы:  
  - `application-dev.properties` (для разработки),  
  - `application-prod.properties` (для продакшена).  
  
>   Активация профиля:  
  ```bash
  java -jar app.jar --spring.profiles.active=prod
  ```

---

### ==**6. Готовые решения для production**==

#### **Spring Boot Actuator:**  

  Позволяет мониторить приложение через HTTP-эндпоинты:  
  - `/health` — статус приложения,  
  - `/metrics` — метрики производительности,  
  - `/info` — информация о версии.  
  
  Для Spring Framework такая функциональность требует самостоятельной реализации.

---

### ==**7. Пример: создание REST-контроллера**==

#### **Spring Framework:**  

  Требует настройки `DispatcherServlet` в `web.xml`,  
  явного включения компонентов через `<context:component-scan>`,  
  и запуска на внешнем сервере.

#### **Spring Boot:**  

  Достаточно:  
  1. Добавить `spring-boot-starter-web`.  
  2. Создать класс с `@SpringBootApplication`.  
  3. Написать контроллер:  
  ```java
  @RestController
  public class DemoController {
      @GetMapping("/hello")
      public String hello() {
          return "Hello, Spring Boot!";
      }
  }
  ```  
  4. Запустить приложение — REST-эндпоинт готов.

---

### ==**Итоговая таблица**==

| **Критерий**         | **Spring Framework**                     | **Spring Boot**                          |
|----------------------|------------------------------------------|------------------------------------------|
| Конфигурация         | Ручная (XML/Java Config)                 | Автоматическая (Convention over Configuration) |
| Зависимости          | Добавляются по отдельности               | Стартеры (готовые наборы)                |
| Сервер               | Внешний (Tomcat/Jetty)                   | Встроенный (Tomcat по умолчанию)         |
| Время настройки      | Больше                                   | Минимум                                  |
| Идеальный сценарий   | Кастомные enterprise-приложения          | Быстрый старт, микросервисы, прототипы   |

---

### ==**Когда что использовать?**==

- **Spring Framework** — если нужен полный контроль над конфигурацией или проект требует нестандартных решений.  

- **Spring Boot** — для быстрого старта, микросервисов, REST API и проектов, где важна скорость разработки.