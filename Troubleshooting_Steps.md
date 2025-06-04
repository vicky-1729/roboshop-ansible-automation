# 🚦 Troubleshooting Steps for Roboshop Application Deployment

Welcome!  
This guide provides clear troubleshooting steps for deploying the Roboshop microservices application. Use these checks after deploying each component to ensure everything is running smoothly.

---

## 🗄️ Database Servers

### 🍃 MongoDB
- **Configuration Check:**  
  Update `/etc/mongod.conf` listen address from `127.0.0.1` to `0.0.0.0`.
- **Service Status:**  
  ```sh
  sudo systemctl status mongodb
  ```
  If not running, check logs for errors.
- **Port Check:**  
  ```sh
  netstat -lntp
  ```
  Ensure port **27017** is listening.
- **Connection Test:**  
  ```sh
  mongosh --host mongodb.<your website>
  show dbs
  use catalogue
  show collections
  db.products.find()
  ```
  *Tip: Install MongoDB shell client if not present.*

---

### 🐬 MySQL
- **Service Status:**  
  ```sh
  sudo systemctl status mysqld
  ```
- **Port Check:**  
  ```sh
  netstat -lntp
  ```
  Ensure port **3306** is listening.
- **Connection Test:**  
  ```sh
  mysql -h mysql.<your website> -uroot -pRoboShop@1
  SHOW DATABASES;
  ```
  *Tip: Ensure the MySQL client is installed.*

---

### 🟥 Redis
- **Configuration Check:**  
  - Change listen address in `/etc/redis/redis.conf` to `0.0.0.0`.
  - Set `protected-mode no`.
- **Service Status:**  
  ```sh
  sudo systemctl status redis
  ```
- **Port Check:**  
  ```sh
  netstat -lntp
  ```
  Ensure port **6379** is listening.

---

### 🐇 RabbitMQ
- **Service Status:**  
  ```sh
  sudo systemctl status rabbitmq
  ```
- **Port Check:**  
  ```sh
  netstat -lntp
  ```
  Ensure port **5672** is listening.

---

## 🛠️ Backend Components

### 📦 Catalogue Service
- **Service Status:**  
  ```sh
  sudo systemctl status catalogue
  ```
  If failed, check MongoDB Route53 record in the service file.
- **Port Check:**  
  ```sh
  netstat -lntp
  ```
  Ensure port **8080** is listening.
- **Connection Test:**  
  ```sh
  telnet <mongodb-route53-record-name> 27017
  mongosh --host mongodb.<your website>
  show dbs
  use catalogue
  show collections
  db.products.find()
  ```

---

### 👤 User Service
- **Service Status:**  
  ```sh
  sudo systemctl status user
  ```
  If failed, check MongoDB and Redis Route53 records in the service file.
- **Port Check:**  
  ```sh
  netstat -lntp
  ```
  Ensure port **8080** is listening.
- **Connection Test:**  
  ```sh
  telnet <mongodb-route53-record-name> 27017
  telnet <redis-route53-record-name> 6379
  ```

---

### 🛒 Cart Service
- **Service Status:**  
  ```sh
  sudo systemctl status cart
  ```
  If failed, check MongoDB Route53 record in the service file.
- **Port Check:**  
  ```sh
  netstat -lntp
  ```
  Ensure port **8080** is listening.
- **Connection Test:**  
  ```sh
  telnet <redis-route53-record-name> 6379
  telnet <catalogue-route53-record-name> 8080
  ```

---

### 🚚 Shipping Service
- **Service Status:**  
  ```sh
  sudo systemctl status shipping
  ```
  If failed, check MySQL Route53 record in the service file.
- **Port Check:**  
  ```sh
  netstat -lntp
  ```
  Ensure port **8080** is listening.
- **Connection Test:**  
  ```sh
  telnet <mysql-route53-record-name> 3306
  mysql -h mysql.<your website> -u root -pRoboShop@1
  SHOW DATABASES;
  ```
  *Tip: Ensure the `cities` schema appears; if not, master data may not be loaded.*

---

### 💳 Payment Service
- **Service Status:**  
  ```sh
  sudo systemctl status payment
  ```
  If failed, check RabbitMQ Route53 record in the service file.
- **Port Check:**  
  ```sh
  netstat -lntp
  ```
  Ensure port **8080** is listening.
- **Connection Test:**  
  ```sh
  telnet <rabbitmq-route53-record-name> 5672
  ```

---

### 🚚 Dispatch Service
- **Service Status:**  
  ```sh
  sudo systemctl status dispatch
  ```
  If failed, check RabbitMQ Route53 record in the service file.
- **Log Check:**  
  After payment on the web page, check for the ID in `/var/log/messages`.

---

## 🌐 Frontend Server

### Nginx
- **Service Status:**  
  ```sh
  sudo systemctl status nginx
  ```
  If failed, check config syntax:  
  ```sh
  nginx -t
  ```
- **Connection Test:**  
  ```sh
  telnet <catalogue-route53-record-name> 8080
  # Example:
  telnet catalogue.<your website> 8080
  ```
- **Restart Nginx:**  
  After deploying backend components:  
  ```sh
  sudo systemctl restart nginx
  ```
- **Health Check:**  
  ```sh
  curl -i http://catalogue.<your website>:8080/health
  curl -i http://cart.<your website>:8080/health
  curl -i http://user.<your website>:8080/health
  curl -i http://shipping.<your website>:8080/health
  curl -i http://payment.<your website>:8080/health
  ```
  **Expected response:**
  ```
  HTTP/1.1 200 OK
  Content-Type: application/json; charset=utf-8
  ```
  If health check fails, verify Route53 records and IP mappings.

---

> **Note:**  
> Replace `royalreddy.site` with `<your website>` wherever applicable.  
> If you find new debugging steps, please add them to this document!

---

