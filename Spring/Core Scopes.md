Scope (область видимости) в Spring определяет **жизненный цикл и контекст**, в котором существует бин. От выбранного scope зависит, как Spring создает, управляет и инжектирует зависимости. Рассмотрим основные виды scope, их особенности и использование.

---

### Основные Scope в Spring

#### 1. **Singleton (по умолчанию)**
- **Описание**: Создается **один экземпляр** на весь контейнер Spring. Все запросы к этому бину возвращают один и тот же объект.
- **Когда использовать**: Для stateless-компонентов (сервисы, утилиты, репозитории).
- **Пример**:
  ```java
  @Component
  @Scope("singleton") // можно не указывать, так как это значение по умолчанию
  public class MySingletonBean { ... }
  ```

#### 2. **Prototype**
- **Описание**: Новый экземпляр создается **при каждом запросе** бина (через `getBean()` или инъекцию).
- **Когда использовать**: Для stateful-компонентов, которые не должны быть общими (например, DTO, обработчики запросов).
- **Пример**:
  ```java
  @Component
  @Scope("prototype")
  public class MyPrototypeBean { ... }
  ```

---

### Web-специфичные Scope
Активируются в веб-приложениях (требуется `spring-web` или `spring-boot-starter-web`).

#### 3. **Request**
- **Описание**: Создается **новый экземпляр** для каждого HTTP-запроса.
- **Когда использовать**: Для данных, специфичных для запроса (например, аутентификация пользователя).
- **Пример**:
  ```java
  @Component
  @Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
  public class MyRequestBean { ... }
  ```

#### 4. **Session**
- **Описание**: Создается **один экземпляр** на HTTP-сессию пользователя.
- **Когда использовать**: Для данных сессии (корзина покупок, настройки пользователя).
- **Пример**:
  ```java
  @Component
  @Scope(value = WebApplicationContext.SCOPE_SESSION, proxyMode = ScopedProxyMode.TARGET_CLASS)
  public class MySessionBean { ... }
  ```

#### 5. **Application**
- **Описание**: Аналог singleton, но привязан к `ServletContext`. Существует на протяжении всего веб-приложения.
- **Когда использовать**: Для глобальных ресурсов (кеш, общие настройки).
- **Пример**:
  ```java
  @Component
  @Scope(value = WebApplicationContext.SCOPE_APPLICATION)
  public class MyApplicationBean { ... }
  ```

#### 6. **WebSocket**
- **Описание**: Создается на время жизни WebSocket-сессии.
- **Когда использовать**: В реальном времени (чаты, уведомления).
- **Пример**:
  ```java
  @Component
  @Scope(value = "websocket", proxyMode = ScopedProxyMode.TARGET_CLASS)
  public class MyWebSocketBean { ... }
  ```

---

### Настройка Scope
- **Через аннотации**:
  ```java
  @Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE) // для prototype
  @Scope(value = WebApplicationContext.SCOPE_REQUEST)     // для request
  ```
- **В XML**:
  ```xml
  <bean id="myBean" class="com.example.MyBean" scope="prototype"/>
  ```

---

### Важные нюансы

#### Проксирование для Web-скопов
Если singleton-бин зависит от request/session-бина, Spring использует **прокси**, чтобы инжектировать актуальный объект при каждом запросе. Для этого указывается `proxyMode`:
```java
@Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
```

#### Проблемы и решения
- **Утечка данных**: Если singleton хранит ссылку на request/session-бин, это может привести к ошибкам. Всегда используйте прокси (`proxyMode`).
- **Производительность**: Prototype-бины создаются чаще, что может нагружать память.

---

### Пример взаимодействия Scope
```java
@Component
@Scope("singleton")
public class SingletonBean {
    @Autowired
    private PrototypeBean prototypeBean; // Каждый раз новый объект?
}

@Component
@Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class PrototypeBean { ... }
```
- Без `proxyMode` в `PrototypeBean`, `SingletonBean` получит один экземпляр `PrototypeBean` на всю жизнь.
- С `proxyMode` Spring внедряет прокси, который при каждом вызове методов `prototypeBean` создает новый экземпляр.

---

### Пользовательские Scope
Можно создавать свои области видимости, реализуя интерфейс `Scope` и регистрируя его в контексте:
```java
public class CustomScope implements Scope { ... }

// Регистрация:
ConfigurableBeanFactory factory = context.getBeanFactory();
factory.registerScope("custom", new CustomScope());
```

---

### Итог
- **Scope** управляет жизненным циклом бинов.
- **Стандартные scopes**: `singleton`, `prototype`.
- **Web-scopes**: `request`, `session`, `application`, `websocket`.
- **Проксирование** решает проблемы зависимостей между бинами разных scope.
- **Аннотации**: `@Scope`, `@RequestScope`, `@SessionScope` (в Spring Boot).