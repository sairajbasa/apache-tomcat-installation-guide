# Apache Tomcat Installation Guide

This repository contains a step-by-step guide for installing and configuring
Apache Tomcat on an Ubuntu EC2 instance. It includes Java installation,
Tomcat setup, permission configuration, startup and shutdown commands,
security group configuration, and verification steps.

---

## 🚀 Project Objective

To provision and configure an Apache Tomcat server on Ubuntu Linux (AWS EC2),
understanding:

- System update & reboot process  
- Java (OpenJDK) installation  
- Downloading & extracting Tomcat  
- Directory & permission configuration  
- Starting and stopping Tomcat  
- Accessing Tomcat using browser (port 8080)  
- Verifying Tomcat service status  

This repository is created as part of hands-on DevOps / Linux practice.

---

## 🛠️ Environment Used

- Platform: AWS EC2  
- OS: Ubuntu Linux  
- Web Server: Apache Tomcat 9  
- Java: OpenJDK 21 (LTS)

---

## 📌 Repository Contents

| File / Folder | Description |
|--------------|----------|
| `README.md` | Project overview & summary |
| `tomcat-setup-guide.md` | Detailed installation steps & commands |
| `screenshots/` | Tomcat UI & AWS screenshots (optional) |

---

## ⚙️ Installation Summary (High Level)

1️⃣ Update system packages  
2️⃣ Install Java (OpenJDK 21)  
3️⃣ Download Apache Tomcat  
4️⃣ Extract and move to `/opt/tomcat`  
5️⃣ Provide execution permissions  
6️⃣ Start Tomcat server  
7️⃣ Access in browser using port `8080`  

Full detailed steps are available here 👇  

👉 **Refer: `tomcat-setup-guide.md`**

---

## 🌐 Accessing Tomcat in Browser
```
http://<EC2-Public-IP>:8080
```

