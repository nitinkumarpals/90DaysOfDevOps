# Day 11 – File Ownership (Linux)

## Overview
This challenge focused on understanding **file ownership**, **groups**, and how Linux manages access using `chown`, `chgrp`, and `sudo`.

---

## Files & Directories Created

### Files
- devops-file.txt
- devops.txt
- notes.txt
- team-notes.txt
- project-config.yaml
- heist-project/vault/gold.txt
- heist-project/plans/strategy.conf

### Directories
- project/
- heist-project/
- heist-project/vault/
- heist-project/plans/

---

## Ownership Changes

### Before → After

- **devops-file.txt**  
  ubuntu:ubuntu → tokyo:ubuntu → berlin:ubuntu

- **team-notes.txt**  
  ubuntu:ubuntu → ubuntu:heist-team

- **project-config.yaml**  
  ubuntu:ubuntu → professor:heist-team

- **heist-project/** (recursive)  
  ubuntu:ubuntu → professor:planners

- **heist-project/vault/gold.txt**  
  ubuntu:ubuntu → professor:planners

- **heist-project/plans/strategy.conf**  
  ubuntu:ubuntu → professor:planners

---

## Commands Used

```bash
# Create files and directories
touch devops-file.txt
touch team-notes.txt
touch project-config.yaml
mkdir -p heist-project/vault
mkdir -p heist-project/plans
touch heist-project/vault/gold.txt
touch heist-project/plans/strategy.conf

# User and group management
sudo adduser berlin
sudo adduser professor
sudo groupadd heist-team
sudo groupadd planners

# Ownership and group changes
sudo chown tokyo devops-file.txt
sudo chown berlin devops-file.txt
sudo chgrp heist-team team-notes.txt
sudo chown professor:heist-team project-config.yaml
sudo chown -R professor:planners heist-project/
```

# Verification
ls -l
ls -ld project/
ls -lR heist-project/
id berlin
id professor

## What I Learned
A Linux file can have only one owner, but multiple users can access it through groups.
Only root (sudo) can change file ownership using chown; normal users can only modify permissions.
Recursive ownership changes require -R, and Linux command options are case-sensitive.
