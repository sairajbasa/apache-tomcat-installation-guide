## 🚀 Provisioning & Configuring Apache Tomcat on Ubuntu (EC2)

This guide walks through installing Java, downloading Apache Tomcat, and starting the Tomcat server on an Ubuntu Linux EC2 instance.

🖥️ Check the Hostname

```
cat /etc/hostname
```

Displays the system hostname to verify the machine identity.

🛠️ Update System Packages
```
sudo apt update && sudo apt upgrade -y
```

apt update → refreshes the package index

apt upgrade -y → installs latest updates automatically

Keeping the system updated ensures stability and security.

🔁 Reboot the System
```
sudo init 6
```

Reboots the instance to apply updates or configuration changes.

⬇️ Install wget
```
sudo apt install wget -y
```

Installs wget, a command-line tool used to download files from the internet.

☕ Install Latest LTS Java (OpenJDK 21)
```
sudo apt install -y openjdk-21-jdk
```

Installs Java 21 LTS, required for running Tomcat.

✔️ (Optional) Install Default Java Package
```
sudo apt install -y default-jdk
```

Installs the default Java version available in Ubuntu repositories
(useful for compatibility on older systems).

📥 Download Apache Tomcat 9
```
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.113/bin/apache-tomcat-9.0.113.tar.gz
```

Downloads the Tomcat 9 compressed archive from the official Apache repository.

📂 List Files in Current Directory
```
ls
```

Verifies that the Tomcat archive has been downloaded successfully.

📦 Extract the Tomcat Archive
```
tar -xvzf apache-tomcat-9.0.113.tar.gz
```

Explanation:

x → extract

v → verbose (shows files being extracted)

z → extract .gz file

f → input file name

This unpacks the Tomcat directory.

🚚 Move Tomcat to /opt/ Directory
```
sudo mv apache-tomcat-9.0.113 /opt/tomcat
```

Moves Tomcat to /opt, a standard location for optional application software.

🔑 Give Execute Permission to Script Files
```
sudo chmod +x /opt/tomcat/bin/*.sh
```

Adds executable permissions to Tomcat startup and shutdown scripts.

▶️ Start Tomcat Server
```
/opt/tomcat/bin/startup.sh
```

Starts the Tomcat service.

📁 Navigate to Tomcat bin Directory
```
cd /opt/tomcat/bin
ls
```

Lists available Tomcat utility scripts.

⏹ Stop Tomcat Server
```
./shutdown.sh
```

Gracefully stops the Tomcat service.

🎯 Next Step (Recommended)

Add sections like:

- Browser Access (http://<EC2-Public-IP>:8080)

- Security Group rule for port 8080

- Tomcat verification commands
