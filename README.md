# SpringBoot_Core

# Spring Boot – Types of Dependency Injection

Dependency Injection (DI) is a design pattern in which Spring provides the required dependencies (objects) to a class instead of the class creating those objects manually.

Spring supports 3 main types of Dependency Injection:

1. Constructor Injection
2. Setter Injection
3. Field Injection

Let's use `UserService` and `UserRepository` for all three examples.

---

# 1. Constructor Injection

### What is Constructor Injection?

In constructor injection, Spring injects the required dependency through the class constructor when creating the object.

### Code Example

**UserRepository.java**

```java
@Repository
public class UserRepository {

    public String getData() {
        return "Data from UserRepository";
    }
}
```

**UserService.java**

```java
@Service
public class UserService {

    private final UserRepository repository;

    public UserService(UserRepository repository) {
        this.repository = repository;
    }

    public void getUsers() {
        System.out.println(repository.getData());
    }
}
```

### How does it work?

1. `@Service` tells Spring to create and manage the UserService bean.
2. `private final UserRepository repository` declares a reference to hold the dependency.
3. Spring identifies the constructor and searches for a matching UserRepository bean.
4. Spring passes the UserRepository bean as a constructor argument.
5. `this.repository = repository` assigns the injected dependency to the instance variable.
6. We can use the repository methods inside UserService.

### Why do we use final?

- Prevents reassignment of the injected dependency reference.
- Once initialized through the constructor, the reference cannot point to another object.
- Helps maintain dependency consistency.
- The repository's internal data can still change.

### Important Points

- Constructor injection is generally recommended in Spring Boot.
- `@Autowired` is optional when the class has only one constructor.
- Supports `final` fields.
- Makes dependencies mandatory during object creation.
- Easier to write unit tests.

---

# 2. Setter Injection

### What is Setter Injection?

In setter injection, Spring injects the dependency by calling a setter method after creating the object.

### Code Example

**UserRepository.java**

```java
@Repository
public class UserRepository {

    public String getData() {
        return "Data from UserRepository";
    }
}
```

**UserService.java**

```java
@Service
public class UserService {

    private UserRepository repository;

    @Autowired
    public void setRepository(UserRepository repository) {
        this.repository = repository;
    }

    public void getUsers() {
        System.out.println(repository.getData());
    }
}
```

### How does it work?

1. `@Service` tells Spring to create and manage the UserService bean.
2. `private UserRepository repository` declares a reference to hold the dependency. Initially, it is null.
3. `@Autowired` tells Spring to inject the dependency through the setter method.
4. Spring searches for a matching UserRepository bean.
5. Spring calls `setRepository()` and passes the repository bean as an argument.
6. `this.repository = repository` assigns the injected dependency to the instance variable.
7. We can now use the repository methods inside UserService.

### Why don't we use final?

- Setter injection happens after object creation.
- A normal final instance variable must be initialized during declaration or inside a constructor.
- We cannot assign a final field inside a normal setter method.
- Without final, the dependency reference can be reassigned through the setter.

### Important Points

- `@Autowired` is generally used to tell Spring which setter method to use for injection.
- Dependency is injected after object creation.
- Supports optional dependencies.
- Does not support normal final instance fields.
- Can be useful when a dependency is optional or needs to be changed through configuration.

---

# 3. Field Injection

### What is Field Injection?

In field injection, Spring directly injects the dependency into the class variable without requiring a constructor or setter method.

### Code Example

**UserRepository.java**

```java
@Repository
public class UserRepository {

    public String getData() {
        return "Data from UserRepository";
    }
}
```

**UserService.java**

```java
@Service
public class UserService {

    @Autowired
    private UserRepository repository;

    public void getUsers() {
        System.out.println(repository.getData());
    }
}
```

### How does it work?

1. `@Service` tells Spring to create and manage the UserService bean.
2. `private UserRepository repository` declares a reference to hold the dependency.
3. `@Autowired` tells Spring to inject the dependency directly into the field.
4. Spring searches for a matching UserRepository bean.
5. Spring injects the repository bean directly into the repository field.
6. We can use the repository methods inside UserService.

### Why don't we use final?

- Spring injects the dependency after object creation.
- A normal final instance variable must be initialized during declaration or inside a constructor.
- Therefore, we cannot use a normal final field with field injection.

### Important Points

- No constructor or setter method is required.
- `@Autowired` is used directly on the field.
- Spring handles the injection internally.
- Does not support normal final instance fields.
- Makes unit testing harder because dependencies are hidden inside the class.
- Generally not recommended for production application design when constructor injection is suitable.

---

# 4. Comparison of All Three Injection Types

| Feature | Constructor | Setter | Field |
|---|---|---|---|
| Injection happens through | Constructor | Setter method | Class field |
| `@Autowired` | Optional for one constructor | Generally required | Required |
| `final` support | Yes | No | No |
| Dependency required at creation | Yes | No | No |
| Optional dependency | Not directly | Yes | Possible |
| Unit testing | Easy | Easy | Harder |
| Recommendation | Preferred | For optional dependencies | Generally avoid |

---

# 5. Real-World Example: When to Use Each?

### Constructor Injection
Use when a dependency is mandatory.

Example: UserService cannot work without UserRepository.

### Setter Injection
Use when a dependency is optional or can be configured after object creation.

Example: An optional notification service.

### Field Injection
Spring supports it, but constructor injection is generally preferred because dependencies are explicit and easier to test.

---

# 6. Important Concepts to Remember

- `@Service` → Registers the service class as a Spring bean.
- `@Repository` → Registers the repository class as a Spring bean and supports persistence exception translation.
- `@Autowired` → Tells Spring to inject a dependency.
- `final` → Prevents reassignment of a reference after initialization.
- `this.repository` → Refers to the instance variable.
- `repository` → Refers to the method or constructor parameter.

**Final Interview Answer:**

Spring supports three main types of dependency injection: Constructor Injection, Setter Injection and Field Injection. Constructor injection is generally preferred because it ensures mandatory dependencies are provided during object creation, supports final fields, and makes the class easier to test. Setter injection is useful for optional dependencies, whereas field injection directly injects dependencies into fields but is generally discouraged.
