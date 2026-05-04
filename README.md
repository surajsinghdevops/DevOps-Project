# 🚀 CI/CD Pipeline Setup: Git → GitHub → Jenkins → Tomcat

> **DevOps Internship Practical Assessment**
> A fully automated CI/CD pipeline — from code commit to live deployment with zero manual intervention.

![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=flat&logo=jenkins&logoColor=white)
![Java](https://img.shields.io/badge/Java-Maven-C71A36?style=flat&logo=apachemaven&logoColor=white)
![Tomcat](https://img.shields.io/badge/Apache%20Tomcat-9-F8DC75?style=flat&logo=apachetomcat&logoColor=black)
![GitHub Webhooks](https://img.shields.io/badge/GitHub-Webhook-181717?style=flat&logo=github&logoColor=white)
![Linux](https://img.shields.io/badge/Ubuntu-Server-E95420?style=flat&logo=ubuntu&logoColor=white)

---

## 📌 Pipeline Overview

```
Developer → git push → GitHub Repo → Webhook Event
                                          ↓
                              Jenkins (Webhook Trigger)
                                          ↓
                              Pull Code → mvn clean package
                                          ↓
                          SSH Deploy → Apache Tomcat → App Live ✅
```

---

## 🛠️ Tools & Versions

| Tool             | Version      | Purpose                        |
|------------------|--------------|--------------------------------|
| Git              | 2.x          | Version control                |
| GitHub           | —            | Remote repo + Webhook trigger  |
| Jenkins          | LTS          | CI/CD automation server        |
| Apache Maven     | 3.8+         | Build tool (WAR packaging)     |
| Apache Tomcat    | 9 / 10       | Application server             |
| Linux / Ubuntu   | 20.04 / 22.04| Server environment             |
| Java (JDK)       | 11+          | Runtime for Jenkins & app      |

---

## 📋 Task 1 — Version Control Setup (Git & GitHub)

### 1.1 Create GitHub Repository

1. Go to [github.com/new](https://github.com/new)
2. Name it `ci-cd-pipeline-demo`
3. Set to **Public**
4. Click **Create repository**

### 1.2 Initialize Local Git Repo & Push

```bash
# Clone or initialize
git init
git remote add origin https://github.com/<YOUR_USERNAME>/ci-cd-pipeline-demo.git

# Add project files
git add .
git commit -m "Initial commit: Java Maven web app"
git push -u origin main
```

### 1.3 Project Structure

```
ci-cd-pipeline-demo/
├── src/
│   └── main/
│       ├── java/com/devops/servlet/
│       │   ├── Student.java
│       │   └── RegistrationServlet.java
│       └── webapp/
│           ├── index.jsp
│           └── WEB-INF/
│               ├── web.xml
│               └── views/
│                   ├── register.jsp
│                   └── success.jsp
├── pom.xml           ← packaging: war
├── .gitignore
├── SETUP.md
└── README.md
```

### 1.4 .gitignore

```
target/
*.class
.idea/
.DS_Store
*.log
```

---

## 📋 Task 2 — Jenkins Installation & Configuration

### 2.1 Install Java (prerequisite)

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
java -version
```

### 2.2 Install Jenkins

```bash
# Add Jenkins repo key
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key \
  | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null

# Add repo
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ \
  | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

# Install
sudo apt update
sudo apt install jenkins -y

# Start & enable
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins
```

Access Jenkins at: `http://<SERVER_IP>:8080`

### 2.3 Unlock Jenkins

```bash
# Get initial admin password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Paste this in the Jenkins setup screen → Install suggested plugins → Create admin user.

### 2.4 Install Maven

```bash
sudo apt install maven -y
mvn -version
```

### 2.5 Install Required Jenkins Plugins

Go to **Manage Jenkins → Plugins → Available plugins** and install:

- ✅ Git Plugin
- ✅ GitHub Integration Plugin
- ✅ Maven Integration Plugin
- ✅ Publish Over SSH Plugin

### 2.6 Configure Maven in Jenkins

**Manage Jenkins → Global Tool Configuration → Maven → Add Maven**

| Field       | Value          |
|-------------|----------------|
| Name        | `Maven-3.8`    |
| Install automatically | ✅  |
| Version     | 3.8.x          |

### 2.7 Add GitHub Credentials

**Manage Jenkins → Credentials → System → Global → Add Credentials**

| Field       | Value                        |
|-------------|------------------------------|
| Kind        | Username with password       |
| Username    | Your GitHub username         |
| Password    | GitHub Personal Access Token |
| ID          | `github-creds`               |

> Generate token at: GitHub → Settings → Developer Settings → Personal Access Tokens → Generate new token (select `repo` scope)

---

## 📋 Task 3 — GitHub Webhook Configuration

### 3.1 Make Jenkins Publicly Accessible

If running locally, use **ngrok**:

```bash
# Install ngrok
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc \
  | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null
echo "deb https://ngrok-agent.s3.amazonaws.com buster main" \
  | sudo tee /etc/apt/sources.list.d/ngrok.list

sudo apt update && sudo apt install ngrok

# Expose Jenkins port
ngrok http 8080
```

Note the public URL: `https://xxxx-xx-xx.ngrok.io`

### 3.2 Add Webhook in GitHub

1. Go to your GitHub repo → **Settings → Webhooks → Add webhook**
2. Fill in:

| Field          | Value                                              |
|----------------|----------------------------------------------------|
| Payload URL    | `http://<JENKINS_IP>:8080/github-webhook/`         |
| Content type   | `application/json`                                 |
| Trigger        | Just the **push event**                            |
| Active         | ✅ Checked                                          |

3. Click **Add webhook**
4. Verify — GitHub should show a ✅ green tick and `200 OK` response

> **If using ngrok:** Payload URL = `https://xxxx-xx-xx.ngrok.io/github-webhook/`

---

## 📋 Task 4 — Jenkins Freestyle Job Setup

### 4.1 Create New Job

**Jenkins Dashboard → New Item → Freestyle project**

Name: `deploy-to-tomcat`

### 4.2 Source Code Management

- Select **Git**
- Repository URL: `https://github.com/<YOUR_USERNAME>/ci-cd-pipeline-demo.git`
- Credentials: Select `github-creds`
- Branch: `*/main`

### 4.3 Build Triggers

✅ Check **GitHub hook trigger for GITScm polling**

> This listens for the incoming webhook from GitHub and triggers the build automatically.

### 4.4 Build Steps

Click **Add build step → Invoke top-level Maven targets**

| Field   | Value           |
|---------|-----------------|
| Maven   | `Maven-3.8`     |
| Goals   | `clean package` |

### 4.5 Save the Job

Click **Save** — the job is now ready to be triggered by a git push.

---

## 📋 Task 5 — Apache Tomcat Setup & SSH Deployment

### 5.1 Install Tomcat on Target Server

```bash
# Install Java first (if not done)
sudo apt install openjdk-11-jdk -y

# Download Tomcat 9
cd /opt
sudo wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.85/bin/apache-tomcat-9.0.85.tar.gz
sudo tar -xzf apache-tomcat-9.0.85.tar.gz
sudo mv apache-tomcat-9.0.85 tomcat

# Set permissions
sudo chmod +x /opt/tomcat/bin/*.sh

# Start Tomcat
sudo /opt/tomcat/bin/startup.sh
```

Access at: `http://<TOMCAT_SERVER_IP>:8080`

### 5.2 Configure Tomcat Manager User

```bash
sudo nano /opt/tomcat/conf/tomcat-users.xml
```

Add inside `<tomcat-users>`:

```xml
<role rolename="manager-script"/>
<role rolename="manager-gui"/>
<user username="admin" password="admin123"
      roles="manager-gui,manager-script"/>
```

Restart Tomcat:

```bash
sudo /opt/tomcat/bin/shutdown.sh
sudo /opt/tomcat/bin/startup.sh
```

### 5.3 Configure SSH Key-Based Authentication

**On Jenkins server:**

```bash
# Generate SSH key pair
ssh-keygen -t rsa -b 4096 -C "jenkins-deploy" -f ~/.ssh/jenkins_deploy
# Press Enter for no passphrase

# View public key
cat ~/.ssh/jenkins_deploy.pub
```

**On Tomcat server:**

```bash
# Add Jenkins public key to authorized_keys
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
# Paste the public key here

chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

**Test SSH connection from Jenkins server:**

```bash
ssh -i ~/.ssh/jenkins_deploy <TOMCAT_USER>@<TOMCAT_SERVER_IP>
```

### 5.4 Configure Publish Over SSH in Jenkins

**Manage Jenkins → Configure System → Publish over SSH**

Click **Add** under SSH Servers:

| Field              | Value                                      |
|--------------------|--------------------------------------------|
| Name               | `tomcat-server`                            |
| Hostname           | `<TOMCAT_SERVER_IP>`                       |
| Username           | `ubuntu` (or your server user)             |
| Remote Directory   | `/opt/tomcat/webapps/`                     |
| Key                | Paste contents of `~/.ssh/jenkins_deploy`  |

Click **Test Configuration** → should show **Success**.

### 5.5 Add Post-Build Action in Jenkins Job

In the `deploy-to-tomcat` job → **Configure → Post-build Actions → Send build artifacts over SSH**

| Field            | Value                                      |
|------------------|--------------------------------------------|
| SSH Server       | `tomcat-server`                            |
| Source files     | `target/*.war`                             |
| Remote directory | `/opt/tomcat/webapps/`                     |
| Exec command     | `sudo /opt/tomcat/bin/shutdown.sh; sleep 3; sudo /opt/tomcat/bin/startup.sh` |

Click **Save**.

---

## 📋 Task 6 — End-to-End Verification

### 6.1 Trigger the Pipeline

Make a visible change to the app (e.g., update a heading in `register.jsp`):

```bash
# Edit a file
nano src/main/webapp/WEB-INF/views/register.jsp

# Commit and push
git add .
git commit -m "test: update heading to trigger pipeline"
git push origin main
```

### 6.2 Verify the Full Chain

| Step | What to check                                    | Expected result     |
|------|--------------------------------------------------|---------------------|
| 1    | GitHub → Settings → Webhooks → Recent Deliveries | ✅ 200 OK           |
| 2    | Jenkins Dashboard → `deploy-to-tomcat`           | 🔵 Build triggered  |
| 3    | Jenkins → Build → Console Output                 | `BUILD SUCCESS`     |
| 4    | Tomcat server webapps/ folder                    | `student-registration.war` present |
| 5    | Browser → `http://<TOMCAT_IP>:8080/student-registration/` | Updated app visible |

### 6.3 Console Output — Expected Success Log

```
Started by GitHub push by <your-username>
Cloning repository https://github.com/<username>/ci-cd-pipeline-demo.git
...
[INFO] Building war: /var/lib/jenkins/workspace/deploy-to-tomcat/target/student-registration.war
[INFO] BUILD SUCCESS
...
SSH: Connecting to <TOMCAT_IP>
SSH: Sending file: target/student-registration.war
SSH: Executing command: sudo /opt/tomcat/bin/shutdown.sh ...
SSH: Finished
Finished: SUCCESS
```

---

## 📦 Deliverables Checklist

- [x] GitHub repo with Java Maven project (`pom.xml` with `<packaging>war</packaging>`)
- [x] `.gitignore` excluding `target/`, `*.class`
- [x] `SETUP.md` — this file
- [ ] `config.xml` — Jenkins job export (see below)
- [ ] Screenshots of Jenkins job config
- [ ] Screenshots of GitHub Webhook `200 OK` delivery
- [ ] Screenshot of Tomcat showing deployed app

### Export Jenkins Job Config

```bash
# On Jenkins server
cat /var/lib/jenkins/jobs/deploy-to-tomcat/config.xml
```

Copy this file into your repo as `jenkins/config.xml`.

---

## ⭐ Bonus Tasks

| Bonus                              | Status     | Notes                                  |
|------------------------------------|------------|----------------------------------------|
| Dockerize Jenkins / Tomcat         | Optional   | Use official Docker images             |
| Jenkinsfile (Pipeline job)         | Optional   | Replace Freestyle with declarative pipeline |
| Build status badge in README       | Easy win ✅ | Add Jenkins badge URL to README        |
| Rollback on failure                | Advanced   | Use `|| sudo /opt/tomcat/bin/startup.sh` in exec command |

### Add Build Status Badge to README

In Jenkins: **Your Job → Embeddable Build Status → Markdown**

Copy and paste at top of your `README.md`:

```markdown
[![Build Status](http://<JENKINS_IP>:8080/buildStatus/icon?job=deploy-to-tomcat)](http://<JENKINS_IP>:8080/job/deploy-to-tomcat/)
```

---

## 🔧 Troubleshooting

| Problem                             | Solution                                                                 |
|-------------------------------------|--------------------------------------------------------------------------|
| Webhook not reaching Jenkins        | Check firewall rules / use ngrok for local setup                         |
| Jenkins build not triggered         | Ensure "GitHub hook trigger for GITScm polling" is checked in job        |
| `mvn: command not found` in Jenkins | Configure Maven under Global Tool Configuration                          |
| SSH connection refused              | Check `authorized_keys` permissions (must be `600`)                      |
| WAR not deployed                    | Check Tomcat `webapps/` path in SSH plugin config                        |
| Tomcat not restarting               | Add `sleep 5` between shutdown and startup in exec command               |
| 403 on Tomcat manager              | Check `tomcat-users.xml` and restart Tomcat                              |

> 💡 **Pro tip from the assessment:** Document every blocker and how you solved it in this file — the evaluation awards marks for showing your troubleshooting process.

---

## 👨‍💻 Author

**Suraj Singh R** — DevOps Engineer & Trainer
- GitHub: [github.com/surajsinghdevops](https://github.com/surajsinghdevops)
- YouTube: [@whatdevops](https://www.youtube.com/@whatdevops)
- Blog: [whatdevops.blogspot.com](http://whatdevops.blogspot.com)

---

*"DevOps is not a goal, but a never-ending process of continual improvement"*
