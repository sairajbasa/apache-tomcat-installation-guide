# 🚀 Provisioning & Configuring Apache Tomcat on Ubuntu (EC2)

This guide walks through installing Java, downloading Apache Tomcat, configuring Tomcat as a **systemd service**, and managing Tomcat using `systemctl` on an Ubuntu Linux EC2 instance.

---

## 🖥️ Verify System Hostname

Displays the system hostname to verify the machine identity.

```bash
hostname
```

---

## 🛠️ Update System Packages

```bash
sudo apt update && sudo apt upgrade -y
```

### Explanation

- `apt update` → Refreshes the package index.
- `apt upgrade -y` → Installs the latest available package updates automatically.

Keeping the system updated helps maintain stability and security.

### 🔄 Reboot the Instance

If required after system updates:

```bash
sudo reboot
```

Reboots the instance to apply updates or configuration changes.

---

## ⬇️ Install wget

```bash
sudo apt install wget -y
```

Installs `wget`, a command-line tool used to download files from the internet.

---

## ☕ Install Java 21 LTS

```bash
sudo apt install -y openjdk-21-jdk
```

Installs OpenJDK 21 LTS, which is required to run Tomcat.

### Verify Java Installation

```bash
java -version
```

Example:

```text
openjdk version "21.x.x"
```

You can also verify the Java installation path:

```bash
readlink -f $(which java)
```

---

## ✔️ Optional: Install Default Java Package

```bash
sudo apt install -y default-jdk
```

Installs the default JDK version available in the Ubuntu repositories.

> **Note:** This step is optional. If Java 21 is already installed and is the version you want to use, you do not need to install `default-jdk`.

---

# 📥 Download Apache Tomcat 9

```bash
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.113/bin/apache-tomcat-9.0.113.tar.gz
```

Downloads the Tomcat 9 compressed archive from the Apache repository.

---

## 📂 List Files in Current Directory

```bash
ls
```

Verifies that the Tomcat archive has been downloaded successfully.

---

## 📦 Extract the Tomcat Archive

```bash
tar -xvzf apache-tomcat-9.0.113.tar.gz
```

### Explanation

- `x` → Extract
- `v` → Verbose output
- `z` → Extract gzip-compressed archive
- `f` → Specifies the input file

This extracts the Tomcat directory.

---

## 🚚 Move Tomcat to `/opt/`

```bash
sudo mv apache-tomcat-9.0.113 /opt/tomcat
```

Moves Tomcat to `/opt`, a standard location for optional application software.

After this step, Tomcat will be located at:

```text
/opt/tomcat
```

---

## 🔑 Give Execute Permission to Tomcat Scripts

```bash
sudo chmod +x /opt/tomcat/bin/*.sh
```

Adds executable permissions to the Tomcat shell scripts.

---

# ▶️ Test Tomcat Manually

Before configuring systemd, you can verify that Tomcat starts successfully.

```bash
/opt/tomcat/bin/startup.sh
```

Expected output:

```text
Using CATALINA_BASE:   /opt/tomcat
Using CATALINA_HOME:   /opt/tomcat
Tomcat started.
```

Verify that Tomcat is listening on port `8080`:

```bash
ss -lntp | grep 8080
```

Stop Tomcat after testing:

```bash
/opt/tomcat/bin/shutdown.sh
```

---

# ⚙️ Configure Tomcat as a systemd Service

Instead of manually running `startup.sh` and `shutdown.sh`, we can configure Tomcat as a Linux service.

This allows us to manage Tomcat using:

```bash
systemctl start tomcat
systemctl stop tomcat
systemctl restart tomcat
systemctl status tomcat
```

It also allows Tomcat to automatically start when the server boots.

---

## 📄 Create `tomcat.service`

Create the systemd service file:

```bash
sudo nano /etc/systemd/system/tomcat.service
```

Add the following configuration:

```ini
[Unit]
Description=Apache Tomcat 9
After=network.target

[Service]
Type=forking

Environment="JAVA_HOME=/usr"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_BASE=/opt/tomcat"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Save and exit the file.

---

## 🔄 Reload systemd

After creating or modifying a service file, reload the systemd configuration:

```bash
sudo systemctl daemon-reload
```

This tells systemd to read the newly created `tomcat.service` file.

---

## 🚀 Enable Tomcat at Boot

```bash
sudo systemctl enable tomcat
```

This configures Tomcat to automatically start when the EC2 instance boots.

---

## ▶️ Start Tomcat Using systemctl

```bash
sudo systemctl start tomcat
```

Tomcat is now managed by systemd.

---

## 🔍 Check Tomcat Service Status

```bash
sudo systemctl status tomcat
```

Expected status:

```text
● tomcat.service - Apache Tomcat 9
     Loaded: loaded (/etc/systemd/system/tomcat.service; enabled)
     Active: active (running)
```

The important part is:

```text
Active: active (running)
```

---

# 🧪 Verify Tomcat Port

Tomcat normally listens on port `8080`.

```bash
sudo ss -lntp | grep 8080
```

You can also test locally:

```bash
curl http://localhost:8080
```

If Tomcat is working, you should receive the Tomcat HTML response.

---

# 🔄 Tomcat Service Management

Once systemd is configured, use `systemctl` instead of directly executing `startup.sh` and `shutdown.sh`.

### Start Tomcat

```bash
sudo systemctl start tomcat
```

### Stop Tomcat

```bash
sudo systemctl stop tomcat
```

### Restart Tomcat

```bash
sudo systemctl restart tomcat
```

### Check Status

```bash
sudo systemctl status tomcat
```

### Check Whether Tomcat Starts Automatically at Boot

```bash
sudo systemctl is-enabled tomcat
```

Expected:

```text
enabled
```

---

# 📋 View Tomcat Logs Through systemd

You can view the service logs using:

```bash
sudo journalctl -u tomcat
```

To follow the logs in real time:

```bash
sudo journalctl -u tomcat -f
```

To view logs from the current boot:

```bash
sudo journalctl -u tomcat -b
```

Tomcat's application logs are also available under:

```text
/opt/tomcat/logs/
```

---

# 🌐 AWS Security Group Configuration

Make sure the EC2 Security Group allows inbound traffic on port `8080`.

Example:

```text
Type: Custom TCP
Port: 8080
Source: Your IP / Required CIDR
```

> **Security recommendation:** For testing, you may temporarily allow `0.0.0.0/0`, but for real environments restrict the source to trusted IP addresses or networks.

---

# 🌍 Browser Access

Once Tomcat is running and the Security Group allows port `8080`, access:

```text
http://<EC2-Public-IP>:8080
```

Example:

```text
http://54.x.x.x:8080
```

You should see the Apache Tomcat welcome page.

---

# 🏁 Final Verification

Run the following commands:

```bash
sudo systemctl status tomcat
```

```bash
sudo systemctl is-enabled tomcat
```

```bash
sudo ss -lntp | grep 8080
```

```bash
curl http://localhost:8080
```

If all four checks are successful, Tomcat is installed, running, and configured to start automatically with the EC2 instance.

---

## 📌 Tomcat Management Summary

| Task | Command |
|---|---|
| Start Tomcat | `sudo systemctl start tomcat` |
| Stop Tomcat | `sudo systemctl stop tomcat` |
| Restart Tomcat | `sudo systemctl restart tomcat` |
| Check status | `sudo systemctl status tomcat` |
| Enable at boot | `sudo systemctl enable tomcat` |
| Disable at boot | `sudo systemctl disable tomcat` |
| View logs | `sudo journalctl -u tomcat` |
| Follow logs | `sudo journalctl -u tomcat -f` |
| Check port | `sudo ss -lntp \| grep 8080` |

---

## 🎯 Final Architecture

```text
                    Ubuntu EC2
                        │
                        ▼
                 ┌──────────────┐
                 │    systemd   │
                 │    Service   │
                 └──────┬───────┘
                        │
                        │ systemctl
                        ▼
              ┌──────────────────┐
              │  tomcat.service  │
              └────────┬─────────┘
                       │
                       ▼
                /opt/tomcat
                       │
                       ▼
              Apache Tomcat 9
                       │
                       ▼
                    Port 8080
                       │
                       ▼
                EC2 Security Group
                       │
                       ▼
                    Internet
                       │
                       ▼
          http://<EC2-Public-IP>:8080
```

**Key concept:** `startup.sh` and `shutdown.sh` are Tomcat's own scripts, while `tomcat.service` is the **systemd unit** that allows Linux to manage Tomcat through `systemctl`.
