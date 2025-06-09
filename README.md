🚀 Full CI/CD Deployment: Java Web App with Jenkins, Nexus & Tomcat

📌 Overview

This project demonstrates an end-to-end DevOps pipeline setup using:
- Version Control – Git + GitHub
- CI/CD Automation – Jenkins
- Build Tool – Maven
- Artifact Storage – Nexus Repository Manager 3
- Deployment – Apache Tomcat 9

The infrastructure spans across three AWS EC2 Linux servers:

| Server         | Purpose             | Stack               |
|----------------|---------------------|---------------------|
| `ci-server`    | Jenkins + Git       | Java 17, Maven, Jenkins |
| `nexus-server` | Artifact Repository | Nexus OSS           |
| `web-server`   | Deployment Host     | Apache Tomcat 9     |

---

🧱 Folder Structure

```
.
├── src/
│   └── main/
│       ├── java/
│       └── webapp/
├── pom.xml
├── Jenkinsfile (optional)
├── README.md
└── results/
    ├── jenkins-success.png
    ├── nexus-artifact.png
    └── tomcat-deploy.png
```

---

⚙️ CI/CD Pipeline Steps

1️⃣ Source Control – Git + GitHub

```bash
git init
git add .
git commit -m "Initial commit with all deployment files"
git remote add origin https://github.com/<your-username>/Jenkins-Deployment-project.git
git push origin master
```

---

2️⃣ Jenkins – Continuous Integration

Install Jenkins and configure Maven + JDK tools:

```linux
sudo yum install java-17-amazon-corretto -y
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum install -y jenkins
sudo systemctl start jenkins
```

Create a freestyle project with GitHub repo as source, and Maven build step:

```bash
Goals: clean package
```

Ensure Maven and Java 17 are configured under Jenkins → Global Tool Configuration.

---

3️⃣ Maven – Build & Deploy

- The `pom.xml` must have proper version and distributionManagement
- Example:
```xml
<version>1.0.1</version>
<distributionManagement>
  <repository>
    <id>nexus</id>
    <url>http://<nexus-ip>:8081/repository/harish/</url>
  </repository>
</distributionManagement>
```

---

4️⃣ Nexus – Artifact Storage
linux
Start Nexus:

sudo systemctl start nexus

Access it at `http://<nexus-ip>:8081`, login with default password, and:

- Create a `maven2 (hosted)` repository named `harish`
- Set deployment policy to "Allow Redeploy"

Artifacts uploaded via `mvn deploy` appear under:

```
http://<nexus-ip>:8081/repository/harish/com/harish/project/viteapp-web/1.0.1/
```

---

5️⃣ Tomcat – Deployment Server

Install Tomcat and configure it to auto-deploy `ROOT.war`:


sudo cp viteapp-web-1.0.1.war /opt/tomcat/webapps/ROOT.war
```

Or use Jenkins post-build step: "Deploy WAR to container" (with correct manager login).

---

🔁 Auto-Deploy Trigger

- Bump version in `pom.xml` (e.g., 1.0.0 → 1.0.1)
- Commit and push to GitHub
- Jenkins job gets triggered
- `.war` is built → stored in Nexus → deployed to Tomcat

---

✅ Results

Screenshots stored in `/results` folder:

- ✅ Jenkins Build Success
- ✅ Nexus Artifact Upload
- ✅ Tomcat App Running

---

## 👤 Author

Harish Maganti
GitHub: [Harishmaganti2](https://github.com/Harishmaganti2)
