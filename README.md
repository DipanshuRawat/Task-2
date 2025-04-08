
# 🚀 Task 2: CI/CD Pipeline for Node.js App using Jenkins and Docker

This task sets up a complete **CI/CD pipeline using Jenkins** to automate the build, test, and push of a Node.js Docker application to Docker Hub.

---

## 📦 Tech Stack

- **Jenkins** (CI/CD tool)
- **Docker** (Containerization)
- **GitHub** (Source Code Management)
- **Docker Hub** (Image Registry)
- **Node.js** (Application)

---

## 📁 Repository

GitHub Repo: [DipanshuRawat/nodejs-demo-app](https://github.com/DipanshuRawat/nodejs-demo-app)

---

## 🔐 DockerHub Credentials Setup (Jenkins)

To push your Docker image securely to Docker Hub, create Jenkins credentials:

1. Open Jenkins → **Manage Jenkins** → **Credentials**
2. Choose: **(global)** > **Add Credentials**
3. Select **"Username with password"**:
   - **Username**: Your Docker Hub username
   - **Password**: Your Docker Hub password or personal access token (PAT)
4. Set the **ID** to:  
   ```
   dockerhub-credentials-id
   ```
5. Save.

---

## 📄 Jenkinsfile

Create a file named `Jenkinsfile` in your GitHub repo.


---

## 🔁 Pipeline Flow (Step-by-Step)

| Stage | Description |
|-------|-------------|
| **Clone** | Pulls latest code from GitHub `main` branch |
| **Build** | Builds Docker image using `Dockerfile` |
| **Test** | Installs dependencies and runs unit tests with `npm test` |
| **Push** | Logs into DockerHub and pushes the image as `username/nodejs-demo-app:latest` |

---

## 🧪 Testing the Pipeline

1. Push any change to the GitHub repo (`main` branch).
2. Jenkins pipeline will be triggered automatically.
3. Watch the logs:
   - ✅ Successful build and test
   - ✅ Docker image gets pushed to Docker Hub

---

## 🐞 Troubleshooting

### 1. ❌ Git Clone Error: `Couldn't find any revision to build`
**Fix:** Ensure you're using:
```groovy
git branch: 'main', url: 'https://github.com/DipanshuRawat/nodejs-demo-app.git'
```

### 2. ❌ Docker Build Error: `invalid reference format`
**Cause:** `docker build -t ****/nodejs-demo-app:latest .`  
**Fix:** Ensure `$USERNAME` is set correctly via `withCredentials`.

### 3. ❌ Docker Socket Permission Denied
**Error:**
```bash
permission denied while trying to connect to the Docker daemon socket
```
**Fix:**
```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

### 4. ❌ DockerHub Login Failed
**Error:**
```bash
unauthorized: incorrect username or password
```
**Fix:**
- Verify DockerHub credentials in Jenkins.
- Use a **Personal Access Token (PAT)** if 2FA is enabled.
- Wrap the login inside `withCredentials`.

---

## ✅ Output

If the pipeline executes successfully:

- ✔ Code cloned from GitHub
- ✔ Docker image built
- ✔ Node.js app tested
- ✔ Image pushed to Docker Hub as:

```
https://hub.docker.com/repository/docker/<your-username>/nodejs-demo-app
```

Example:
```
dipanshurawat/nodejs-demo-app:latest
```

Run to verify:
```bash
docker pull dipanshurawat/nodejs-demo-app:latest
```

---

## 🧼 Clean-up Commands (Optional)

```bash
docker rmi dipanshurawat/nodejs-demo-app:latest
```

---

## 🙌 Conclusion

You now have a fully automated pipeline that builds, tests, and pushes your app to Docker Hub whenever changes are made to the repo. This is the foundation of any DevOps workflow.

---
