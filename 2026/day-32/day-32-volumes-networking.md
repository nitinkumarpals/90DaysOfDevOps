# Day 32 – Docker Volumes & Networking

## Task 1: The Problem (No Volume)

### Run MySQL Container
```
docker run -d --name mysql-test -e MYSQL_ROOT_PASSWORD=root123 mysql
```

### Create Data
Connected and created sample database/table.

### Remove Container
```
docker stop mysql-test
docker rm mysql-test
```

### Run New Container
```
docker run -d --name mysql-test2 -e MYSQL_ROOT_PASSWORD=root123 mysql
```

### Result
❌ Data was lost.

### Reason
Containers are ephemeral. Without volumes, data is deleted when container is removed.

---

## Task 2: Named Volumes

### Create Volume
```
docker volume create db-data
```

### Run Container with Volume
```
docker run -d --name mysql-vol -v db-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root123 mysql
```

### Remove Container
```
docker stop mysql-vol
docker rm mysql-vol
```

### Run Again with Same Volume
```
docker run -d --name mysql-vol2 -v db-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root123 mysql
```

### Result
✅ Data persisted.

### Verify
```
docker volume ls
docker volume inspect db-data
```

---

## Task 3: Bind Mounts

### Create Folder
```
mkdir mysite
echo "Hello from Docker" > mysite/index.html
```

### Run Nginx
```
docker run -d -p 8080:80 -v $(pwd)/mysite:/usr/share/nginx/html nginx
```

### Result
Website accessible at:

http://localhost:8080

Editing index.html updates website instantly.

### Difference

Named Volume:
- Managed by Docker
- Stored in Docker directory
- Best for databases

Bind Mount:
- Uses host folder
- Easy to edit files
- Best for development

---

## Task 4: Docker Networking Basics

### List Networks
```
docker network ls
```

### Inspect Bridge
```
docker network inspect bridge
```

### Run Containers
```
docker run -dit --name c1 alpine sh
docker run -dit --name c2 alpine sh
```

### Ping by Name
```
ping c2
```

Result:

❌ Failed

### Ping by IP
```
ping 172.17.0.x
```

Result:

✅ Success

Reason:

Default bridge network does not support DNS resolution.

---

## Task 5: Custom Networks

### Create Network
```
docker network create my-app-net
```

### Run Containers
```
docker run -dit --name app1 --network my-app-net alpine sh
docker run -dit --name app2 --network my-app-net alpine sh
```

### Ping by Name
```
ping app2
```

Result:

✅ Success

### Reason

Custom networks provide automatic DNS resolution between containers.

---

## Task 6: Put It Together

### Create Network
```
docker network create app-net
```

### Create Volume
```
docker volume create db-data2
```

### Run Database
```
docker run -d --name mysql-db --network app-net -v db-data2:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root123 mysql
```

### Run App Container
```
docker run -dit --name app-container --network app-net alpine sh
```

### Test Connection
```
ping mysql-db
```

Result:

✅ Successful

---

## Key Learnings

- Containers lose data without volumes
- Named volumes persist data
- Bind mounts link host files
- Default bridge has no DNS
- Custom networks support DNS
