# 🕵️‍♂️ Dockerized Web Scraper with MySQL Integration

This project demonstrates how to build a **Dockerized web scraper** using Python that scrapes movie quotes from a website and stores the data in a **MySQL database** running in a separate Docker container. The setup uses a **custom Docker network** with the `bridge` driver for seamless container communication.

---

## 🚀 Features
- **Web Scraping**: Uses Python's `requests` and `BeautifulSoup` to extract data.
- **MySQL Database**: Stores scraped data in a separate MySQL container.
- **Dockerized Setup**: Uses Docker for easy deployment and environment consistency.
- **Custom Network**: Ensures secure and efficient communication between containers.

---

## 🛠️ Prerequisites
Ensure you have the following installed before proceeding:
- **Docker** (for containerized environment)
- **Python 3** (if you wish to run the scraper outside of Docker)
- Python packages: `requests`, `BeautifulSoup`, `mysql-connector-python`

---

## 📌 Setup & Installation

### **1️⃣ Create a Custom Docker Network**
Run the following command to create a network for container communication:
```sh
docker network create --driver bridge scraper_network
```

### **2️⃣ Run MySQL Container**
Deploy a MySQL container and attach it to the `scraper_network`:
```sh
docker run -d \
    --name mysql_container \
    -e MYSQL_ROOT_PASSWORD=redhat \
    -e MYSQL_DATABASE=scraper_db \
    --network scraper_network \
    -p 3306:3306 \
    mysql:latest
```

### **3️⃣ Access MySQL & Create Table**
Connect to the MySQL container:
```sh
docker exec -it mysql_container mysql -u root -predhat
```
Select the database:
```sql
USE scraper_db;
```
Create the table for storing quotes:
```sql
CREATE TABLE quotes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    text TEXT NOT NULL,
    author VARCHAR(255) NOT NULL
);
```

### **4️⃣ Build the Web Scraper Image**
```sh
docker build -t scraper-app:v1 .
```

### **5️⃣ Run the Web Scraper Container**
```sh
docker run -d \
    -e DB_HOST=mysql_container \
    -e DB_USER=sayantan \
    -e DB_PASSWORD=redhat \
    -e DB_NAME=scraper_db \
    --network scraper_network \
    scraper-app:v1
```

---

## ⚠️ Important Notes
- The `--name mysql_container` in **Step 2** must match `DB_HOST=mysql_container` in **Step 5**.
- The `DB_NAME=scraper_db` must be consistent across both MySQL and Scraper configurations.
- If you encounter connection issues, ensure that **both containers are running** and part of the same network.

---

## ✅ Verifying the Setup
Check running containers:
```sh
docker ps
```
View logs of the scraper container:
```sh
docker logs <scraper-container-id>
```
Connect to MySQL and verify data:
```sql
SELECT * FROM quotes;
```

---

## 📖 Conclusion
This setup allows you to scrape and store data efficiently using Dockerized containers. Feel free to extend the project by adding features like **cron jobs for periodic scraping**, **data processing pipelines**, or **API integration** for real-time access.

🚀 **Happy Scraping!** 🕵️‍♂️

