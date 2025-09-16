# INSTRUCTION.md

## Overview

This project contains two Dockerized components:

1. A **MySQL database** container based on the official MySQL image.
2. A **Django TodoApp** container that connects to the MySQL database.

Follow the steps below to build, run, and access the application.

---

## 1. Run MySQL container with a volume attached

First, pull or build the prepared MySQL image.

```bash
# Build the image locally (if not already built)
docker build -t mysql-local:1.0.0 -f Dockerfile.mysql .

# OR pull from your personal Docker Hub
docker pull kenu21/mysql-local:1.0.0
```

Run the MySQL container with a volume for persistent storage:

```bash
docker run -d \
  --name mysql-local \
  -e MYSQL_DATABASE=app_db \
  -e MYSQL_USER=app_user \
  -e MYSQL_PASSWORD=1234 \
  -e MYSQL_ROOT_PASSWORD=root \
  -v mysql_data:/var/lib/mysql \
  -p 3306:3306 \
  kenu21/mysql-local:1.0.0
```

* `mysql_data` is a named Docker volume used to persist the database.
* The container exposes MySQL on port **3306**.

---

## 2. Run the TodoApp container connected to MySQL

Build or pull the Django application image:

```bash
# Build locally
docker build -t todoapp:2.0.0 .

# OR pull from Docker Hub
docker pull kenu21/todoapp:2.0.0
```

Now run the TodoApp container and link it to the MySQL container:

```bash
docker run \
  --name todoapp-container \
  -p 8000:8080 \
  kenu21/todoapp:2.0.0
```

### Notes:

* `<mysql-container-ip>` can be retrieved by running:

  ```bash
  docker network inspect bridge
  ```

---

## 3. Access the Application

Once both containers are running:

* Open your browser and go to:
  👉 [http://localhost:8000](http://localhost:8000)

* You should see the **Django Todolist App** landing page.

---

## 4. Docker Hub Repositories

* **MySQL Image:**
  [https://hub.docker.com/r/kenu21/mysql-local](https://hub.docker.com/r/<your-dockerhub-username>/mysql-local)

* **TodoApp Image:**
  [https://hub.docker.com/r/kenu21/todoapp](https://hub.docker.com/r/<your-dockerhub-username>/todoapp)

---

## 5. Summary of Commands

```bash
# Run MySQL
docker run -d --name mysql-local \
  -e MYSQL_DATABASE=app_db \
  -e MYSQL_USER=app_user \
  -e MYSQL_PASSWORD=1234 \
  -e MYSQL_ROOT_PASSWORD=root \
  -v mysql_data:/var/lib/mysql \
  -p 3306:3306 \
  kenu21/mysql-local:1.0.0

# Run TodoApp
docker run -d --name todoapp \
  -p 8000:8000 \
  kenu21/todoapp:2.0.0
```
