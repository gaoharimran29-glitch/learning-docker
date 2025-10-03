# Docker Compose Learning Journey

## Index
- [Task 1: Hello World with Docker Compose](#task-1-hello-world-with-docker-compose)
- [Task 2: Nginx Web Server](#task-2-nginx-web-server)
- [Task 3: Python Simple HTTP Server](#task-3-python-simple-http-server)
- [Task 4: MySQL Database with Volumes](#task-4-mysql-database-with-volumes)
- [Task 5: Running Python and MySQL Together](#task-5-running-python-and-mysql-together)

---

## Task 1: Hello World with Docker Compose

**Goal:** Run a simple container using Docker Compose.

### docker-compose.yml
```yaml
version: '3.8'
services:
  hello:
    image: hello-world
```

### Explanation
- Uses the official `hello-world` image.  
- When run, it prints a "Hello from Docker!" message.  

**Run:**
```bash
docker-compose up
```

---

## Task 2: Nginx Web Server

**Goal:** Serve a static web page using Nginx.

### docker-compose.yml
```yaml
version: '3.8'
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
```

### Explanation
- **`nginx:alpine`** → lightweight Nginx image.  
- Maps host port **8080 → container 80**.  
- Mounts local `./html` folder to `/usr/share/nginx/html` in container.  

**Run:**  
```bash
docker-compose up -d
```
Open browser → `http://localhost:8080`

---

## Task 3: Python Simple HTTP Server

**Goal:** Run a Python HTTP server inside a container.

### docker-compose.yml
```yaml
version: '3.8'
services:
  web:
    image: python:3.9
    working_dir: /app
    volumes:
      - .:/app
    command: python3 -m http.server 8000
    ports:
      - "8000:8000"
```

### Explanation
- Uses `python:3.9` official image.  
- Mounts current folder (`.`) into `/app` inside container.  
- Runs `python3 -m http.server 8000` to serve files.  
- Maps **8000 → 8000** so server is available at `http://localhost:8000`.

---

## Task 4: MySQL Database with Volumes

**Goal:** Run a MySQL DB with persistent storage.

### docker-compose.yml
```yaml
version: '3.8'
services:
  db:
    image: mysql:8.0
    container_name: my_mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: testdb
      MYSQL_USER: testuser
      MYSQL_PASSWORD: testpass
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```

### Explanation
- Runs MySQL 8.0 server.  
- Exposes port `3306` for host connections.  
- Creates a default database (`testdb`) and user (`testuser`).  
- Uses `db_data` named volume to persist data.  

**Run:**
```bash
docker-compose up -d
```

**Connect from host:**
```bash
mysql -h 127.0.0.1 -P 3306 -u testuser -p
```
Password = `testpass`

**Or inside container:**
```bash
docker exec -it my_mysql mysql -u root -p
```
Password = `rootpassword`

---
## Task5: Running python and MySQL together

```docker
version: '3.8'

services:
  myphonebook:
    build: ./        
    container_name: myphonebook
    networks:
      - my-net
    depends_on:
      mydb:
        condition: service_healthy

  mydb:
    image: mysql:9.4
    restart: always
    container_name: mydbsql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: phonebook
    networks:
      - my-net
    ports:
      - "3306:3306"
    volumes:
      - db-data:/var/lib/mysql
    healthcheck:                      
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 20s

volumes:
  db-data:

networks:
  my-net:
```
## Explanation:
- Used heathcheck so that when MySQL is ready for connection then python will try to make connection to not get an error.