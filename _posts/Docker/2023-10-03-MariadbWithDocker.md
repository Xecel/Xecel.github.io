---
title: (Docker) Mariadb 시작하기
date: 2023-10-03 00:00:00 +0900
lastmod: 2023-10-03 00:00:00 +0900
categories: [Docker, DB]
tags: [Docker, DB]
---

<style>
.env-text {
  color: #0f52ba;
}
.command-text {
  color: #8B00FF;
}
</style>

## 개발 환경

- <span class="env-text">M1 Mac</span>
- <span class="env-text">Docker version 24.0.2</span>

## Mariadb with Docker

**Docker를 사용해 Mariadb 사용법을 알아보자**

![1_pull_image](/assets/img/2023-10-03/1_pull_image.jpg)
**_<code class="command-text">docker pull mariadb</code>_**

```
docker pull mariadb
```

**docker hub에서 mariadb 이미지를 pull 해줍니다.**

---

![2_images](/assets/img/2023-10-03/2_images.jpg)
**_<code class="command-text">docker images</code>_**

```
docker images
```

**pull 한 mariadb 이미지가 나타납니다.**

---

![3_docker_run](/assets/img/2023-10-03/3_docker_run.jpg)
**_<code class="command-text">docker run --name my-mariadb -e MYSQL_ROOT_PASSWORD=myPassword -p 3306:3306 -d mariadb</code>_**

```
docker run --name my-mariadb -e MYSQL_ROOT_PASSWORD=myPassword -p 3306:3306 -d mariadb
```

**`docker run` : Docker container를 실행합니다..**

**`--name my-mariadb` : container 이름을 "my-mariadb"로 부여합니다.**

**`--name my-mariadb` : container 이름을 "my-mariadb"로 부여합니다.**

**`-e MYSQL_ROOT_PASSWORD=myPassword` : Mariadb의 환경변수에 password를 할당하여 root user의 비밀번호를 변경합니다.**

**`-p 3306:3306` : 호스트(외부)의 3306포트를 내부의 3306포트로 매핑하여 외부에서 Docker mariadb를 사용할 수 있습니다.**

**`-d mariadb` : 컨테이너를 백그라운드 모드로 실행합니다.**

---

![4_docker_ps](/assets/img/2023-10-03/4_docker_ps.jpg)
**_<code class="command-text">docker ps</code>_**

```
docker ps
```

**현재 실행 중인 Docker Container들을 표시합니다.**

---

![5_docker_exec](/assets/img/2023-10-03/5_docker_exec.jpg)
**_<code class="command-text">docker exec -it (mariadb_container_name) bash</code>_**

```
docker exec -it (mariadb_container_name) bash
```

**Mariadb Container 내부의 Bash Shell에 접속합니다.**

---

**_bash 쉘에 진입한 다음부터는 SQL 쿼리 명령어를 사용합니다._**

---

![6_show_tables](/assets/img/2023-10-03/6_show_tables.jpg)
**_<code class="command-text">show databases;</code>_**

```
show databases;
```

**현재 모든 데이터베이스 목록을 표시합니다.**

**현재는 MariaDB 시스템에 관련된 데이터베이스만 있는 상태입니다.**

---

![7_create_database](/assets/img/2023-10-03/7_create_database.jpg)
**_<code class="command-text">show databases;</code>_**

```
create database (DB_name);
```

**새로운 데이터베이스를 생성합니다.**

---

![8_use_database](/assets/img/2023-10-03/8_use_database.jpg)
**_<code class="command-text">use (DB_name);</code>_**

```
use (DB_name);
```

**해당 데이터베이스를 사용합니다.**

---

![9_create_table](/assets/img/2023-10-03/9_create_table.jpg)
**_<code class="command-text">create_table;</code>_**

```
CREATE TABLE Account (
account_id INT AUTO_INCREMENT
PRIMARY KEY,
username VARCHAR(50) NOT NULL,
email VARCHAR(100) UNIQUE NOT NULL,
password VARCHAR(255) NOT NULL,
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**임의의 Account 테이블을 생성합니다.**

---

![10_insert_data](/assets/img/2023-10-03/10_insert_data.jpg)
**_<code class="command-text">insert into (table_name) ~</code>_**

```
INSERT INTO Account (username, email, password)
VALUES
('john_doe', 'john@example.com', 'password123'),
('jane_smith', 'jane@example.com', 'securepwd456'),
('bob_johnson', 'bob@example.com', 'strongpass789');
```

**Account 테이블에 임의의 데이터를 삽입합니다.**

---

![11_show_data](/assets/img/2023-10-03/11_show_data.jpg)
**_<code class="command-text">SELECT ∗ FROM Account;</code>_**

```
SELECT * FROM Account;
```

**삽입된 데이터를 확인할 수 있습니다.**
