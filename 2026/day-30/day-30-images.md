# Day 30 – Docker Images & Container Lifecycle

## Docker Images

### Pull Images
```
docker pull nginx
docker pull ubuntu
docker pull alpine
```

### List Images
```
docker images
```

Observation:
- nginx ≈ 60MB
- ubuntu ≈ 70MB+
- alpine ≈ 7MB

### Ubuntu vs Alpine

Ubuntu:
- Full Linux distribution
- Larger size
- More tools

Alpine:
- Minimal Linux
- Very small
- Faster downloads

Reason Alpine is smaller:
- Uses BusyBox
- Minimal libraries
- Designed for containers

---

## Inspecting Images

Command:

```
docker inspect nginx
```

Information Found:

- Image ID
- OS = linux
- Architecture = amd64
- Size ≈ 63MB
- Exposed Port = 80
- Environment variables
- Entrypoint and CMD
- Image Layers

---

## Image Layers

Command:

```
docker image history nginx
```

Layers represent:

- Base OS
- Installed packages
- Copied files
- Commands

Why Docker Uses Layers:

1. Faster builds
2. Layer caching
3. Shared storage
4. Faster downloads

Some layers show 0B because they only contain metadata.

---

## Container Lifecycle

Lifecycle:

Created → Running → Paused → Stopped → Removed

### Created

```
docker create nginx
```

- Container exists
- Not running

### Running

```
docker start nginx
```

- Container active
- Application running

### Paused

```
docker pause nginx
```

- Container frozen
- No CPU usage

### Stopped

```
docker stop nginx
```

- Container stopped
- Can restart

### Restart

```
docker restart nginx
```

Flow:

Running → Stopped → Running

Restart is an operation, not a lifecycle state.

### Removed

```
docker rm nginx
```

- Container deleted
- Cannot restart

---

## docker kill vs docker rm

docker kill:

```
docker kill nginx
```

- Force stop container
- Container still exists

docker rm:

```
docker rm nginx
```

- Delete container
- Must be stopped first

Stop and remove together:

```
docker rm -f nginx
```

---

## Working with Containers

Run Container:

```
docker run -d -p 80:80 --name nginx nginx
```

View Logs:

```
docker logs nginx
```

Real-time Logs:

```
docker logs -f nginx
```

Exec into Container:

```
docker exec -it nginx bash
```

Run Single Command:

```
docker exec nginx ls /
```

Inspect Container:

```
docker inspect nginx
```

Important Fields:

Container ID:
```
"Id": "..."
```

State:
```
"State": {...}
```

IP Address:
```
"IPAddress": "172.17.0.2"
```

Mounts:
```
"Mounts": []
```

---

## Cleanup Commands

Stop all containers:

```
docker stop $(docker ps -q)
```

Remove all containers:

```
docker rm $(docker ps -aq)
```

Remove unused images:

```
docker image prune -a
```

Check disk usage:

```
docker system df
```

---

## Key Learnings

Today I learned:

- Docker images vs containers
- Image layers
- Container lifecycle
- docker inspect
- docker kill vs docker rm
- Container cleanup

Docker images are templates.
Containers are running instances of images.
