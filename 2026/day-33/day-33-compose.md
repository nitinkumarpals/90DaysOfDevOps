# Day 33 – Docker Compose: Multi-Container Basics

## Task 1: Install & Verify

### Check Docker Compose
```
docker compose version
```

Result:
Docker Compose is installed and working.

---

## Task 2: First Compose File

### Folder
```
compose-basics/
```

### docker-compose.yml
```yaml
services:
  nginx:
    image: nginx
    ports:
      - "8080:80"
```

### Start
```
docker compose up -d
```

### Access
http://<server-ip>:8080

### Stop
```
docker compose down
```

---

## Task 3: Two Container Setup

### docker-compose.yml

```yaml
services:

  mysql:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wp123
    volumes:
      - mysql-data:/var/lib/mysql

  wordpress:
    image: wordpress
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: mysql:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wp123
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - mysql

volumes:
  mysql-data:
```

### Start Services
```
docker compose up -d
```

### Result

WordPress accessible in browser.

### Persistence Test

```
docker compose down
docker compose up -d
```

Result:

Data persisted because of named volume.

---

## Task 4: Compose Commands

### Start Detached
```
docker compose up -d
```

### View Running Services
```
docker compose ps
```

### View Logs
```
docker compose logs
```

### Specific Service Logs
```
docker compose logs mysql
```

### Stop Without Removing
```
docker compose stop
```

### Start Again
```
docker compose start
```

### Remove Everything
```
docker compose down
```

### Remove Volumes Also
```
docker compose down -v
```

### Rebuild Images
```
docker compose up -d --build
```

Note:
Rebuild works only when Dockerfile is used.

---

## Task 5: Environment Variables

### .env File

```
MYSQL_ROOT_PASSWORD=root123
MYSQL_DATABASE=wordpress
MYSQL_USER=wpuser
MYSQL_PASSWORD=wp123
```

### docker-compose.yml

```yaml
services:

  mysql:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
    volumes:
      - mysql-data:/var/lib/mysql

  wordpress:
    image: wordpress
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: mysql:3306
      WORDPRESS_DB_USER: ${MYSQL_USER}
      WORDPRESS_DB_PASSWORD: ${MYSQL_PASSWORD}
      WORDPRESS_DB_NAME: ${MYSQL_DATABASE}

volumes:
  mysql-data:
```

### Verify Variables

```
docker compose config
```

```
docker exec mysql-wp env | grep MYSQL
```

Result:

Environment variables successfully loaded.

---

## Key Learnings

- Docker Compose runs multi-container apps easily
- Compose automatically creates networks
- Service names act as DNS
- Volumes persist data
- .env files manage configuration
