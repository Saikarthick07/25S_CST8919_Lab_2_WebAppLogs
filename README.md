# CST8919 Lab 2: Building a Web App with Threat Detection using Azure Monitor and KQL

## 🎥 YouTube Demo

Youtube Demo Link: https://youtu.be/4HxNLOUnecw 


# 🔐 Flask Login Monitor – Azure Security Lab

## 🚀 Overview

This project is a demonstration of securing a simple Python Flask web application by integrating it with **Azure App Service**, **Azure Monitor**, and **Log Analytics** to detect and alert on suspicious login activities (e.g., brute-force attacks).

The app logs both successful and failed login attempts, and we use **Kusto Query Language (KQL)** to analyze the logs and create an **alert rule** when brute-force patterns are detected.

---

## 🛠️ Technologies Used

- Python Flask
- Azure App Service
- Azure Log Analytics
- Azure Monitor Alerts
- KQL (Kusto Query Language)
- VS Code REST Client (`.http` file for testing)

---

## 📂 Project Structure

flask-login-monitor/
│
├── app.py # Flask application
├── requirements.txt # Python dependencies
├── test-app.http # VS Code REST Client test script
└── README.md # Project documentation


---

## 📦 Setup Instructions

1. **Create Flask App** (`app.py`)
2. **Deploy to Azure App Service**
3. **Enable Diagnostic Logs**
4. **Generate Test Logs** using `test-app.http`
5. **Query Logs with KQL**
6. **Create Azure Monitor Alert Rule**

---

## 🧪 Testing the App

Test login attempts with the `test-app.http` file via REST Client extension in VS Code:

```http
### Successful login
POST https://<your-app-name>.azurewebsites.net/login
Content-Type: application/json

{
  "username": "admin",
  "password": "password123"
}

### Failed login
POST https://<your-app-name>.azurewebsites.net/login
Content-Type: application/json

{
  "username": "admin",
  "password": "wrongpass"
}
```

## 📊 KQL Query for Detecting Failed Logins

```
AppServiceConsoleLogs
| where TimeGenerated > ago(30m)
| where Message contains "Failed login attempt"
```
## Explanation:

- AppServiceConsoleLogs: The log table that captures application logs.
- TimeGenerated > ago(30m): Filters logs from the last 30 minutes.
- Message contains "Failed login attempt": Finds failed login attempts by matching log message content.

## 🚨 Azure Monitor Alert Rule

Configuration:

- Scope: Log Analytics Workspace
- Condition:
Custom log query using above KQL
- Threshold: Greater than 5 results
- Granularity: 5 minutes
- Frequency: 1 minute
- Action Group: Sends email alert
- Severity: 2 (High)
