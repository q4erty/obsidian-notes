

---

### 1. `@SpringBootApplication`

**Что делает?**  
Эта аннотация отмечает **главный класс** приложения Spring Boot. Она объединяет три ключевые аннотации:
- `@Configuration`: Класс может содержать конфигурационные бины (методы с `@Bean`).
- `@EnableAutoConfiguration`: Включает автоматическую настройку Spring Boot (например, автонастройку Tomcat, Spring MVC).
- `@ComponentScan`: Сканирует текущий пакет и подпакеты на наличие компонентов (контроллеры, сервисы и т.д.).

**Пример:**
```java
@SpringBootApplication
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args); // Запуск приложения
    }
}
```

**Почему важно?**  
Она запускает весь механизм Spring Boot, избавляя вас от ручной настройки.

---

### 2. `@RestController`

**Что делает?**  
Помечает класс как **контроллер**, который обрабатывает HTTP-запросы. Объединяет:
- `@Controller`: Указывает, что класс является контроллером.
- `@ResponseBody`: Возвращаемые значения методов записываются напрямую в тело HTTP-ответа (а не в шаблон HTML).

**Пример:**
```java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, World!";
    }
}
```
При запросе `GET /hello` клиент получит строку `"Hello, World!"`.

---

### 3. `@RequestMapping`

**Что делает?**  
Связывает HTTP-запросы (по пути и методу) с методом контроллера. Можно использовать на уровне класса или метода.

**Параметры:**
- `value` (или `path`): URL-путь (например, `/users`).
- `method`: HTTP-метод (GET, POST и т.д.).

**Пример:**
```java
@RestController
@RequestMapping("/api")
public class UserController {
    
    @RequestMapping(value = "/users", method = RequestMethod.GET)
    public List<User> getUsers() {
        // Возвращает список пользователей
    }
}
```
Этот метод обрабатывает `GET /api/users`.

**Современные аналоги:**  
- `@GetMapping("/path")` → `@RequestMapping(method = GET)`
- `@PostMapping`, `@PutMapping` и т.д.

---

### 4. `@Autowired

**Что делает?**  
Внедряет зависимости (бин Spring) автоматически. Spring находит подходящий бин и присваивает его полю/конструктору/методу.

**Где использовать:**
- На полях (не рекомендуется для production).
- На конструкторах (лучшая практика).
- На сеттерах.

**Пример через конструктор:**
```java
@RestController
public class UserController {
    private final UserService userService;

    // Spring автоматически внедрит UserService
    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/users")
    public List<User> getUsers() {
        return userService.getAllUsers();
    }
}
```

**Важно:**  
- В Spring 4.3+ для конструкторов с одним параметром `@Autowired` можно не указывать.
- Убедитесь, что класс `UserService` помечен как `@Service` или другой Spring-стереотип.

---

### Как это работает вместе?
1. **`@SpringBootApplication`** запускает приложение и сканирует компоненты.
2. **`@RestController`** помечает класс как обработчик HTTP-запросов.
3. **`@RequestMapping`** определяет, какой метод вызывать для конкретного URL.
4. **`@Autowired`** внедряет сервисы в контроллер, чтобы разделить логику.