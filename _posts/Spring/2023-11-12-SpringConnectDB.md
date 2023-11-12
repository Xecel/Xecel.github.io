---
title: (Spring) Mariadb와 연결하기
date: 2023-11-12 00:00:00 +0900
lastmod: 2023-11-12 00:00:00 +0900
categories: [Spring, DB]
tags: [Spring, DB]
---

<style>
.env-text {
  color: #0f52ba;
}
.command-text {
  color: #8B00FF;
}
</style>

<style>
blockquote {
  display: block;
  background-color: #f8f9fa;
  color: #000080;
}
</style>

**Spring에서 MariaDB와 연결하는 방법**

IntelliJ에서 Spring Initializar를 사용하여 기본적인 포맷을 만든 상태에서 Docker에서 실행중인 MariaDB와 연결하는 방법을 알아보겠습니다.

---

```
implementation 'org.mariadb.jdbc:mariadb-java-client:2.7.3'
```

JDBC driver 의존성을 추가해줍니다.

---

```
spring:
  datasource:
    // MariaDB 데이터베이스의 URL 및 포트 지정
    url: jdbc:mysql://localhost:3306/springDB
    // 사용자 이름
    username: root
    // 사용자 패스워드
    password: mypassword
    // 사용하는 드라이버의 클래스 이름
    driver-class-name: org.mariadb.jdbc.Driver
  jpa:
    hibernate:
      // DB의 테이블을 자동 생성
      ddl-auto: create
```

> application.yml 파일에서는 DB 연결 관련 옵션을 설정합니다.
>
> **url**은 현재 실행 중인 Docker의 포트에 매핑되며, 포트 뒤에는 /(DB 이름)이 붙습니다.
>
> **ddl-auto: create** 서버 시작 시 정의한 엔티티를 DB 테이블에 자동으로 생성해 주는데, update, validate 등 여러 옵션이 있습니다. 하지만 배포 과정에선 이 옵션을 주의해서 사용해야 합니다.

---

**위 과정이 끝나면 서버와 DB간의 연결은 완료됬다고 볼 수 있습니다.**

**이하 코드는 실제로 연결됬는지 테스트 하는 코드입니다.**

```java

// User.java

@Entity(name = "User")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column
    private String username;

    @Column
    private String email;
}

```

```java

// UserRepository.java

public interface UserRepository extends JpaRepository<User, Long> {
}

```

```java

// UserService.java

@RequiredArgsConstructor
@Service
public class UserService {
    private final UserRepository userRepository;

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public User createUser(User user) {
        return userRepository.save(user);
    }
}

```

```java

// UserController.java

@RequiredArgsConstructor
@RestController
public class UserController {
    private final UserService userService;

    @GetMapping("/get")
    public List<User> getUser() {
        return userService.getAllUsers();
    }

    @PostMapping("/create")
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }
}

```

---

**간단하게 username, email를 필드로 갖는 Entity를 생성하고 create, read 기능 확인하는 코드를 생성했습니다.**

---

![1_db](/assets/img/2023-11-12/1_db.jpg)

**서버를 시작하면 user 테이블이 생성됩니다.**

---

![2_post](/assets/img/2023-11-12/2_post.jpg)

**postman을 사용해서 임의의 json data를 post 해줍니다.**

---

![3_select_from](/assets/img/2023-11-12/3_select_from.jpg)

**DB에 저장됩니다.**

---

![4_get](/assets/img/2023-11-12/4_get.jpg)

**get 사용 시 조회 가능합니다.**
