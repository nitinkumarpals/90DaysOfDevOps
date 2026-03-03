# Day 38 -- YAML Basics

## Task 1 -- Key Value Pairs

### person.yaml

``` yaml
name: Nitin Kumar
role: DevOps Learner
experience_years: 1
learning: true
```

------------------------------------------------------------------------

## Task 2 -- Lists

### Updated person.yaml

``` yaml
name: Nitin Kumar
role: DevOps Learner
experience_years: 1
learning: true

tools:
  - Docker
  - Kubernetes
  - GitHub Actions
  - Terraform
  - Linux

hobbies: [coding, learning, reading]
```

### Two Ways to Write Lists in YAML

1)  Block Style

``` yaml
tools:
  - Docker
  - Kubernetes
  - Terraform
```

2)  Inline Style

``` yaml
hobbies: [coding, learning, reading]
```

------------------------------------------------------------------------

## Task 3 -- Nested Objects

### server.yaml

``` yaml
server:
  name: production-server
  ip: 192.168.1.10
  port: 8080

database:
  host: localhost
  name: appdb

  credentials:
    user: admin
    password: secret123
```

------------------------------------------------------------------------

## Task 4 -- Multi-line Strings

``` yaml
startup_script_pipe: |
  #!/bin/bash
  echo "Starting server"
  docker start myapp
  systemctl restart nginx

startup_script_fold: >
  This script starts the server
  and ensures all services
  are running properly.
```

### When to use \| vs \>

-   `|` preserves line breaks (used for scripts).
-   `>` folds text into a single paragraph (used for descriptions).

------------------------------------------------------------------------

## Task 5 -- Validation

Install:

``` bash
sudo apt install yamllint -y
```

Validate:

``` bash
yamllint person.yaml
yamllint server.yaml
```

------------------------------------------------------------------------

## Task 6 -- Spot the Difference

Correct:

``` yaml
name: devops
tools:
  - docker
  - kubernetes
```

Broken:

``` yaml
name: devops
tools:
- docker
  - kubernetes
```

Issue: Incorrect indentation of list items.

------------------------------------------------------------------------

## Key Learnings

1.  YAML uses spaces only (no tabs).
2.  Indentation is critical (2 spaces standard).
3.  YAML supports strings, numbers, booleans, lists, and nested objects.

------------------------------------------------------------------------

## Folder Structure

    2026/day-38/
    ├── person.yaml
    ├── server.yaml
    └── day-38-yaml.md

------------------------------------------------------------------------

## Git Commands

``` bash
git add 2026/day-38/
git commit -m "Day 38 - YAML Basics"
git push
```
