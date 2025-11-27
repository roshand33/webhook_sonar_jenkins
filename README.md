# webhook_sonar_jenkins
# SonarQube → Jenkins Webhook Integration Guide

This **README.md** explains how to configure a webhook in **SonarQube** to trigger **Jenkins** once code analysis is complete — including the screenshots you provided.

---

## 📸 Screenshots

### 1. SonarQube Admin Panel

![Screenshot 115](Screenshot%20\(115\).png)

### 2. Administration → Configuration → Webhooks

![Screenshot 116](Screenshot%20\(116\).png)

### 3. Create Webhook Popup

![Screenshot 117](Screenshot%20\(117\).png)

### 4. Webhook List View

![Screenshot 118](Screenshot%20\(118\).png)

### 5. Webhook Detail Popup

![Screenshot 119](Screenshot%20\(119\).png)

### 6. Final Webhook Confirmation

![Screenshot 120](Screenshot%20\(120\).png)

> ⚠️ **Note:** When uploading to GitHub, place all images inside the repository root or `/assets/` folder. Update image paths accordingly.

---

## ✅ Prerequisites

Ensure the following before creating the webhook:

* SonarQube server running (e.g., `http://13.201.222.107:9000`)
* Jenkins server running (e.g., `http://13.201.222.107:8080`)
* SonarQube Scanner & Plugins configured in Jenkins
* Jenkins pipeline uses the `waitForQualityGate` step

---

## 📌 Step 1: Login to SonarQube Admin

* Open SonarQube
* Login as `admin`
* Open **My Account → Administration**

---

## 📌 Step 2: Open Webhooks Settings

Navigate to:

```
Administration → Configuration → Webhooks
```

Then click **Create**.

---

## 📌 Step 3: Create Webhook for Jenkins

Use the following details:

* **Name:** `jenkins`
* **URL:**

```
http://13.201.222.107:8080/sonarqube-webhook/
```

**Important:** Avoid incorrect formatting like:

```
http://http://13.201.222.107:8080/sonarqube-webhook/
```

Leave **Secret** empty.

---

## 📌 Step 4: Validate Webhook Delivery

* After running an analysis, return to Webhooks page
* Under **Last Delivery**, you should see a green ✔ if successful

---

## 📌 Step 5: Configure Jenkins Pipeline to Receive Webhook

Add this to your Jenkins pipeline:

```groovy
stage('Quality Gate') {
    steps {
        script {
            waitForQualityGate abortPipeline: false
        }
    }
}
```

This makes Jenkins pause until the SonarQube webhook responds.

---

## 🚀 Workflow Summary

1. Jenkins builds project
2. Jenkins triggers SonarQube scan
3. SonarQube completes analysis
4. SonarQube sends webhook to Jenkins:

```
http://<jenkins-ip>:8080/sonarqube-webhook/
```

5. Jenkins resumes pipeline after receiving Quality Gate result

---

## 🛠 Common Issues & Fixes

| Issue                          | Cause                        | Fix                     |
| ------------------------------ | ---------------------------- | ----------------------- |
| Webhook delivery failed        | Wrong URL                    | Correct URL formatting  |
| Jenkins didn't respond         | Jenkins offline              | Start Jenkins service   |
| Pipeline stuck at Quality Gate | Missing `waitForQualityGate` | Add step in Jenkinsfile |
| Duplicate `http://`            | Typo                         | Remove extra prefix     |

---

## 📄 Ready for GitHub

This README.md can be directly pushed to your GitHub repo.
If you'd like:

* Auto-create folder structure (`/assets/screenshots`)
* Auto-upload images
* Generate final GitHub repo layout

Just tell me!
