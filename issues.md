# Docker Issues

If Jenkins is saying **"Docker image is offline"**, it could be due to several reasons. Here are the possible causes and solutions:  

---

### **🔍 Possible Causes & Fixes**  

#### **1️⃣ Docker Daemon is Not Running**  
**Check if Docker is running on the Jenkins node:**  
```sh
systemctl status docker
```
If it's not running, start it:  
```sh
sudo systemctl start docker
```
To ensure it starts on boot:  
```sh
sudo systemctl enable docker
```

---

#### **2️⃣ Jenkins User Lacks Docker Permissions**  
Jenkins might not have permission to access Docker. Run:  
```sh
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```
Then log out and log back in.

---

#### **3️⃣ Docker Image Not Available Locally**  
If the pipeline references an image that isn't available, pull it manually:  
```sh
docker pull my-image:latest
```
Or update the pipeline to always pull the latest image:  
```groovy
agent {
    docker {
        image 'my-image:latest'
        alwaysPull true
    }
}
```

---

#### **4️⃣ No Internet Access or Docker Hub Issues**  
Test connectivity by running:  
```sh
ping google.com
```
If there's no connection, check your network settings.  
If Docker Hub is down, try pulling from a different registry.

---

#### **5️⃣ Proxy or Firewall Blocking Docker**  
If you're behind a corporate proxy, configure Docker to use the proxy:  
```sh
sudo mkdir -p /etc/systemd/system/docker.service.d
echo "[Service]
Environment=\"HTTP_PROXY=http://your.proxy:port/\"
Environment=\"HTTPS_PROXY=http://your.proxy:port/\"
" | sudo tee /etc/systemd/system/docker.service.d/http-proxy.conf
sudo systemctl daemon-reload
sudo systemctl restart docker
```

---

#### **6️⃣ Docker-in-Docker (DinD) Issues**  
If using Docker inside Jenkins running in a container, ensure it has access to the host Docker daemon:  
```yaml
volumes:
  - /var/run/docker.sock:/var/run/docker.sock
```
Or use **Docker-in-Docker (DinD)**:  
```yaml
image: docker:dind
privileged: true
```

---

### **🔹 Next Steps**
- Check the exact error message in Jenkins logs (`/var/log/jenkins/jenkins.log`).  
- Try running the pipeline manually with debugging enabled.  
- Let me know if you need more details! 🚀
