




### Spring Data JPA: Репозитории и Аннотации `@Entity` и `@Table`

Spring Data JPA — это модуль Spring, который упрощает работу с базами данных через ORM-фреймворк Hibernate. Он позволяет работать с базой данных, используя объектно-реляционное отображение (ORM), а также предоставляет удобный API для выполнения операций CRUD.

---

## 1. Аннотация `@Entity`

Аннотация `@Entity` используется для указания, что класс представляет собой сущность базы данных. Это обязательная аннотация для любого класса, который должен быть сохранен в БД.

Пример:

```java
import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity
public class User {
    @Id
    private Long id;
    private String name;
    private String email;

    // Геттеры и сеттеры
}
```

- Класс `User` становится сущностью (Entity), которую Hibernate может связать с таблицей в базе данных.
- Поле `id` помечено как `@Id`, что означает, что оно является первичным ключом.

---

## 2. Аннотация `@Table`

Аннотация `@Table` позволяет указать, к какой таблице в БД должна быть привязана сущность. Она используется, если имя таблицы отличается от имени класса.

Пример:

```java
import jakarta.persistence.*;

@Entity
@Table(name = "users_table")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "user_name", length = 50, nullable = false)
    private String name;

    @Column(unique = true)
    private String email;

    // Геттеры и сеттеры
}
```

- `@Table(name = "users_table")` указывает, что данная сущность будет храниться в таблице `users_table`.
- `@Column(name = "user_name")` указывает, что поле `name` в Java соответствует колонке `user_name` в БД.

---

## 3. Репозитории в Spring Data JPA

Spring Data JPA позволяет работать с БД через **репозитории**, которые являются абстракцией над DAO-слоем.

### 3.1 Интерфейс `JpaRepository`

Spring Data JPA предоставляет интерфейс `JpaRepository<T, ID>`, который содержит готовые методы для выполнения операций CRUD.

Пример:

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
}
```

- `JpaRepository<User, Long>` означает, что репозиторий работает с сущностью `User`, а идентификатор у нее типа `Long`.
- Репозиторий уже включает методы, такие как:
    - `findAll()`
    - `findById(Long id)`
    - `save(User user)`
    - `deleteById(Long id)`

---

### 3.2 Создание кастомных запросов

Помимо стандартных методов, можно определять свои запросы с помощью `@Query` или именованных методов.

#### Поиск по имени:

```java
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.Optional;

public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByName(String name);
}
```

Spring Data JPA автоматически сгенерирует SQL-запрос:

```sql
SELECT * FROM users_table WHERE user_name = ?;
```

#### Использование `@Query`:

```java
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import java.util.List;

public interface UserRepository extends JpaRepository<User, Long> {
    @Query("SELECT u FROM User u WHERE u.name LIKE %:keyword%")
    List<User> searchByName(@Param("keyword") String keyword);
}
```

Этот запрос ищет всех пользователей, чьи имена содержат `keyword`.

---

## 4. Пример работы с БД в Spring Boot

### 4.1 Конфигурация `application.properties`

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mydatabase
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

### 4.2 Сервисный слой

```java
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public void createUser(User user) {
        userRepository.save(user);
    }
}
```

### 4.3 Контроллер

```java
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/users")
public class UserController {
    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping
    public List<User> getUsers() {
        return userService.getAllUsers();
    }

    @PostMapping
    public void addUser(@RequestBody User user) {
        userService.createUser(user);
    }
}
```

---

## Итог

- `@Entity` — делает класс сущностью JPA.
- `@Table(name = "...")` — привязывает сущность к конкретной таблице в БД.
- `JpaRepository<T, ID>` — предоставляет готовые CRUD-методы.
- Можно определять кастомные методы поиска через `findByXxx()` или `@Query`.
- Контроллер и сервисный слой помогают структурировать код.

Этот базовый набор позволит тебе эффективно работать с базами данных в Spring Boot с минимальными усилиями. 🚀