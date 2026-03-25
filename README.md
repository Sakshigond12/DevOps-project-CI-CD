# CI/CD Pipeline with Automated Docker Security Scanning and Deployment to Kubernetes  

---

# 📌 Project Overview

This project demonstrates a secure and automated CI/CD pipeline using Jenkins, Docker, Trivy, and Kubernetes. The goal is to ensure that only secure and vulnerability-free Docker images are deployed to production.

Whenever code is pushed to GitHub, Jenkins triggers the pipeline to build a Docker image, push it to Docker Hub, and scan it using Trivy. If HIGH or CRITICAL vulnerabilities are detected, the pipeline fails automatically. Only secure images are deployed to Kubernetes.

This project follows DevSecOps practices by integrating security directly into the CI/CD workflow.


---

#  🔄 CI/CD Pipeline Workflow

```bash
Developer → GitHub → Jenkins → Docker → Trivy → Kubernetes
```
- Code pushed to GitHub  
- Jenkins pipeline triggered  
- Docker image built  
- Image pushed to Docker Hub  
- Trivy scans image  
   - ❌ If vulnerabilities → Pipeline FAIL  
   - ✅ If secure → Deploy to Kubernetes  

---


# 🏗️ Architecture

![Architecture](https://github.com/Sakshigond12/DevOps-project-CI-CD/blob/1d100b2cbaa69a1596755d79fa7e28e3a7b35ca1/screenshots/Architechture%20diagram.png)

---


# 🛠️ Technologies Used

- Docker  
- Jenkins  
- Kubernetes (Minikube)  
- Trivy  
- GitHub  

---

# ☁️ AWS EC2 Setup

### 🔹Launch EC2 Instance

- Go to AWS Console  
- Open EC2 Dashboard  
- Click on **Launch Instance**  
- Configure:
   - AMI: Ubuntu  
   - Instance Type: t3.medium
   - EBS volumes size: 30 GiB
   - Key Pair: Create or select existing 
   - Launch the instance 

 ### 🔐 Security Group Configuration

| Type        | Port Range       | Description              |
|-------------|----------------|------------------------------|
| SSH         | 22             | EC2 access                   |
| HTTP        | 80             | Web access                   |
| Custom TCP  | 8080           | Jenkins                      |
| Custom TCP  | 3000           | Application                  |
| Custom TCP  | 30007          | Kubernetes NodePort          | 
| Custom Tcp  | 50000          | Jenkins internal comunication|

 

---

### 🔹 Connect to EC2

```bash
ssh -i your-key.pem ubuntu@your-ec2-public-ip
```
---


# ⚙️ Installation Commands Used

### 🐳 Docker Installation
```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
docker --version
```

---

### 🔧 Jenkins Installation
```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
java --version

sudo docker pull jenkins/jenkins:lts
13 sudo docker run -d -p 8080:8080 -p 50000:50000 --name jenkins jenkins/jenkins:lts

sudo apt update
sudo systemctl enable jenkins
```
---

### 🔐 Trivy Installation
```bash
sudo apt install wget apt-transport-https gnupg lsb-release -y

wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -

echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | \
sudo tee /etc/apt/sources.list.d/trivy.list

sudo apt update
sudo apt install trivy -y

trivy --version
```

---

### ☸️ Minikube Installation
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

minikube start
```

---

### 📦 kubectl Installation
```bash
sudo apt install -y kubectl
kubectl version --client
```

---

# 📁 Project Structure

```
DevOps-project-CI-CD/
│
├── myapp/
│   ├── Dockerfile
|   |── Jenkinsfile
|   ├── trivy-report.txt
|   ├── docker_build_log.txt
│   ├── package.json (Node.js app)
│   ├── service.js 
|   ├── app.js
│   ├── kubernetes/
│       ├── deployment.yaml
│       └── service.yaml
│
├── Screenshots
|   ├── Architechture diagram.png
│   ├── Docker_container_status.png
│   ├── Dockerhub_image_pushed.png
│   ├── EC2_instance.png
│   ├── Jenkins_Pipeline_Success.png
│   ├── Kubernetes_Pods_and_Service_Runnig.png
│   ├── Pipeline_Failed_Due_To_Trivy_Scan.png
│   ├── Security_group_add.png
│   └── Trivy_Vulnerability_Report.png
|
├── README.md
```

---

# 🐳 Push Docker Image to Docker Hub

### 🔹 Step 1: Login to Docker Hub
```bash
docker login
```

---

### 🔹 Step 2: Build Docker Image
```bash
docker build -t myapp:latest ./myapp
```

---

### 🔹 Step 3: Tag Image
```bash
docker tag myapp:latest your-dockerhub-username/myapp:latest
```

---

### 🔹 Step 4: Push Image
```bash
docker push your-dockerhub-username/myapp:latest
```

---

### 🔹 Step 5: Verify Image

- Go to Docker Hub  
- Check your repository  
- Image should be visible  

---


# ☸️ Kubernetes Deployment Verification

### 🔹 Check Running Pods
```bash
kubectl get pods
```

---

### 🔹 Check Services
```bash
kubectl get svc
```

---

### 🔹 Deploy Application (if not deployed)
```bash
kubectl apply -f myapp/kubernetes/deployment.yaml
kubectl apply -f myapp/kubernetes/service.yaml
```

---

### 🔹 Access Application

```bash
minikube service <service-name>
```

---

### 🔹 Expected Output

- Pods status: **Running**  
- Service: **Active**  
- Application accessible via browser  

---


# 🔐 Trivy Security Scan

### 🔹 Scan Docker Image
```bash
trivy image your-dockerhub-username/myapp:latest
```

---

### 🔹 Save Scan Report (Text)
```bash
trivy image -f table -o trivy-report.txt your-dockerhub-username/myapp:latest
```

---

### 🔹 Save Scan Report (JSON)
```bash
trivy image -f json -o trivy-report.json your-dockerhub-username/myapp:latest
```

---

### 🔹 Expected Output

- List of vulnerabilities (LOW, MEDIUM, HIGH, CRITICAL)  
- Summary of total vulnerabilities  
- Fixed versions (if available)  

---

### 🔹 Pipeline Behavior

- ❌ If HIGH/CRITICAL vulnerabilities → Pipeline FAIL  
- ✅ If no critical issues → Deployment allowed  

---


# 🔄 Jenkins Pipeline

### 🔹 Pipeline Execution

- Code pushed to GitHub  
- Jenkins automatically triggers pipeline  
- Pipeline executes following stages:

---

### 🔹 Pipeline Stages

- 📥 Clone Code  
- 🐳 Build Docker Image  
- 📤 Push Image to Docker Hub  
- 🔐 Trivy Security Scan  
- 🚀 Run Docker Container  
- ☸️ Deploy to Kubernetes  

---

### 🔹 Run Pipeline

- Open Jenkins Dashboard  
- Select your pipeline project  
- Click **Build Now**  
- Open **Console Output** to monitor execution  

---

### 🔹 Expected Result

- ✅ All stages successful → Application deployed  
- ❌ Vulnerabilities found → Pipeline failed  

---

# 🔐 Security Implementation

- Trivy image scanning  
- Pipeline security gate  
- Secure deployment to Kubernetes  
- Automated vulnerability detection  

---


# 📸 Screenshots

---

### ☁️ EC2 Instance
![EC2](https://github.com/Sakshigond12/DevOps-project-CI-CD/blob/1d100b2cbaa69a1596755d79fa7e28e3a7b35ca1/screenshots/EC2_instance.png)

---

### 🔐 Security Group Configuration
![Security Group](https://github.com/Sakshigond12/DevOps-project-CI-CD/blob/1d100b2cbaa69a1596755d79fa7e28e3a7b35ca1/screenshots/Security_group_add.png)

---

### ❌ Pipeline Failure (Trivy)
![Pipeline Fail](https://github.com/Sakshigond12/DevOps-project-CI-CD/blob/1d100b2cbaa69a1596755d79fa7e28e3a7b35ca1/screenshots/Pipeline_Failed_Due_To_Trivy_Scan.png)

---

### 🔄 Jenkins Pipeline Success
![Jenkins](https://github.com/Sakshigond12/DevOps-project-CI-CD/blob/1d100b2cbaa69a1596755d79fa7e28e3a7b35ca1/screenshots/Jenkins_Pipeline_Success.png)

---

### 🔐 Trivy Vulnerability Report
![Trivy](https://github.com/Sakshigond12/DevOps-project-CI-CD/blob/1d100b2cbaa69a1596755d79fa7e28e3a7b35ca1/screenshots/Trivy_Vulnerability_Report.png)

---

### 🐳 Docker Container Running
![Docker](https://github.com/Sakshigond12/DevOps-project-CI-CD/blob/1d100b2cbaa69a1596755d79fa7e28e3a7b35ca1/screenshots/Docker_container_status.png)

---

### 📦 Docker Hub Image
![DockerHub](https://github.com/Sakshigond12/DevOps-project-CI-CD/blob/1d100b2cbaa69a1596755d79fa7e28e3a7b35ca1/screenshots/Dockerhub_image_pushed.png)

---

### ☸️ Kubernetes Pods & Service
![Kubernetes](https://github.com/Sakshigond12/DevOps-project-CI-CD/blob/1d100b2cbaa69a1596755d79fa7e28e3a7b35ca1/screenshots/Kubernetes_Pods_and_Service_Runnig.png)

--- 


# 🧠 Key Learnings

- CI/CD pipeline automation  
- Docker containerization  
- Security scanning using Trivy  
- Kubernetes deployment  
- DevSecOps practices  

---

# 🎉 Conclusion

This project successfully implements a secure CI/CD pipeline where vulnerable images are blocked and only secure applications are deployed, ensuring reliability and security in the deployment process.

---

# 👩‍💻Author

**Sakshi Gond**

Aspiring Cloud & DevOps Engineer  

🔗 GitHub: https://github.com/Sakshigond12  

🔗 LinkedIn: https://linkedin.com/in/sakshi-gond-9ab9092a2
