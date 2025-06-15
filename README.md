
## 🧱 2. Setup & Run SonarQube

Download the SonarQube version included in the repository (or download [SonarQube 8.9 LTS](https://www.sonarsource.com/products/sonarqube/downloads/)).

### ▶️ Start SonarQube (based on OS)

```bash
cd sonarqube-8.9.10.61524/bin/{your-os-folder}
./sonar.sh start
```

✅ **Ensure you're using Java 11** in your terminal/session when running SonarQube.

---

## 🌐 3. Configure SonarQube Dashboard

1. Open your browser and go to: [http://localhost:9000](http://localhost:9000)
2. Log in using the default credentials:
   - **Username:** `admin`
   - **Password:** `admin` (you may be prompted to change this)
3. Click **"Create Project"** → **"Manually"**
4. Enter:
   - **Project Key** (e.g., `test`)
   - **Display Name**
5. Generate a **Token** for CLI authentication and keep it safe.

---

## 📊 4. Analyze Java Code with Sonar Scanner (Maven)

Navigate to the root of your Java project and run the following command:

```bash
mvn sonar:sonar \
  -Dsonar.projectKey={yourProjectKey} \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login={yourGeneratedToken}
```

✅ Your project should now appear on the SonarQube dashboard.
![image](https://github.com/user-attachments/assets/4adcf61b-503c-4983-b672-684923441edc)

---

## 🧩 5. Run the Node.js Service

This service provides CWE, CVE, CERT, and OWASP mapping and enrichment.

```bash
cd node-service
npm install
node index.js
```

⚠️ **Make sure the port used in this Node.js project (default: 3000) is free and not in conflict with other services.**

---

## 🚦 6. Run the Java Spring Boot Application

This is the main backend that calculates and exposes attack surface metrics via REST API.

```bash
cd attack-surface-metrics
./mvnw spring-boot:run
```

App will be available at: [http://localhost:8080](http://localhost:8080)

---

## 📡 API Usage

### 🔍 Endpoint

```http
GET /getAllIssues?projectKey={yourProjectKey}
```

- Replace `{yourProjectKey}` with the actual project key used in SonarQube.

---

## 🧾 Sample Response

```json
{
  "finalScore": 0.023333333333333334,
  "unweightedScore": 56.0,
  "allRules": [
    {
      "key": "java:S1874",
      "name": "\"@Deprecated\" code should not be used",
      "title": {
        "cwe": ["CWE-477"],
        "CertList": ["CERT, MET02-J."],
        "owasp": []
      },
      "consequenceList": [
        {
          "Scope": ["Other"],
          "Impact": ["Other"],
          "Note": null
        }
      ],
      "consequencScore": 4,
      "cveScore": 0.0,
      "score": 8.0,
      "effectiveCWE": "710",
      "severity": "MINOR",
      "cvelist": ["CVE-2021-33528"]
    },
    {
      "key": "java:S1104",
      "name": "Class variable fields should not have public accessibility",
      "title": {
        "cwe": ["CWE-493"],
        "CertList": [],
        "owasp": []
      },
      "consequenceList": [
        {
          "Scope": ["Other"],
          "Impact": ["Other"],
          "Note": null
        }
      ],
      "consequencScore": 4,
      "cveScore": 0.0,
      "score": 8.0,
      "effectiveCWE": "664",
      "severity": "MINOR",
      "cvelist": ["CVE-2016-8763", "CVE-2019-5816"]
    },
    {
      "key": "java:S106",
      "name": "Standard outputs should not be used directly to log anything",
      "title": {
        "cwe": [],
        "CertList": ["CERT, ERR02-J."],
        "owasp": []
      },
      "consequenceList": [],
      "consequencScore": 0,
      "cveScore": 1.0,
      "score": 0.0,
      "effectiveCWE": null,
      "severity": "MAJOR",
      "cvelist": null
    }
  ]
}
```

---

## 🧮 Score Definitions

- **`finalScore`**: Weighted attack surface score based on severity, CVE/CWE metadata, and other metrics.
- **`unweightedScore`**: Raw score from SonarQube rule hits without weighting.
- **`allRules`**: A list of all Sonar rules triggered along with:
  - Sonar Rule Key & Name
  - Mapped CWE, CVE, CERT data
  - Severity
  - Consequence score
  - Effective CWE
  - Rule impact score

---

## 📝 Troubleshooting

- **SonarQube fails to start?**  
  Ensure you are using **Java 11**. Java 17+ is not supported for SonarQube 8.9.

- **macOS warning: `libwrapper.dylib`**  
  Safe to ignore as long as the SonarQube UI loads properly.

- **Elasticsearch failure (Security Manager error)?**  
  Downgrade Java to 11, as Java 17+ removes `SecurityManager` which breaks compatibility.

---

## 📄 License

This project is for **academic/research** use only. License details coming soon.

---

## 🤝 Contributions

Pull requests are welcome!  
If you want to propose major changes, please open an issue first to discuss the direction.

---

## 🙋‍♂️ Maintainer

**Yudeep Rajbhandari**

---

Let me know if you'd like to:

- Add GitHub Actions (CI/CD) for automatic testing or analysis
- Integrate with SonarCloud for public metrics
- Package the system using Docker for easier deployment
