# 🛠️ Issues & Fixes Log

A living log of environment quirks, dev bumps, and how we tamed them — battle-tested during setup of the **Stock Market Insight Platform**.

---

## 1. PostgreSQL: Python Authentication Failed

**Error:**
```
psycopg2.OperationalError: password authentication failed for user 'admin'
```

**Cause:**  
Port 5432 was hijacked by a locally installed PostgreSQL instance, not the Dockerized container.

**Fix:**
- Uninstalled local Postgres completely (via Control Panel/services)
- Restarted Docker container
- Verified successful connection with `validateDBConnection.py`

---

## 2. Java Version Mismatch

**Issue:**  
System was running Java 11, but project required Java 21.

**Fix:**
- Installed Java 21 LTS
- Updated `JAVA_HOME` and system `Path`
- Verified upgrade with:
  ```bash
  java -version
  ```

---

## 3. MongoDB Authentication Failed in DBGate/mongosh

**Error:**
```
MongoServerError: Authentication failed
```

**Root Causes:**
- `.env` not loaded — credentials weren’t applied
- Connection string missing `authSource=admin`
- Mongo container was already initialized without a root user

**Fixes:**
- Inlined credentials directly in `docker-compose.database.yml`
  ```yaml
  MONGO_INITDB_ROOT_USERNAME: admin
  MONGO_INITDB_ROOT_PASSWORD: password
  ```
- Used correct connection URI:
  ```
  mongodb://admin:password@localhost:27017/?authSource=admin
  ```
- Reset Mongo container:
  ```bash
  docker-compose down -v
  docker-compose up -d
  ```

---

## 4. Mongo CLI Not Found

**Issue:**  
Local `mongo`/`mongosh` CLI not installed

**Fix:**  
Used container shell to run the CLI:
```bash
docker exec -it mongo_service mongosh -u admin -p password --authenticationDatabase admin
```

---

## 5. Docker Volume Confusion (Persistence)

**Concern:**  
Would Mongo/Postgres/Redis data be wiped when shutting down containers or laptop?

**Resolution:**
- Switched from bind mounts to **named volumes** (e.g. `mongo_data:`)
- Named volumes are:
  - Persistent across reboots and container restarts
  - Docker-managed and portable
  - Only wiped with `docker-compose down -v`

---

## 6. .env Not Used by Teammates

**Issue:**  
Some environments didn’t pick up the `.env` file, causing missing credentials.

**Fix:**  
- Removed reliance on `.env`
- Hardcoded credentials directly into `docker-compose.database.yml` under `environment:` for each service

---

## ✅ General Debugging Tips

- Use `docker logs <service>` to inspect container startup and user creation issues
- Use `docker exec -it <container> bash` (or `mongosh`) for interactive checks
- Rebuild cleanly with:
  ```bash
  docker-compose down -v && docker-compose up -d
  ```

---

_Keep appending to this file as the platform evolves — each bug here earns future-you a debugging shortcut._