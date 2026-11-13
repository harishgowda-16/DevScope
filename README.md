# DevScope (DevPort) - Developer Portfolio & Resume Analyzer

DevScope is a full-stack Spring Boot application that analyzes a developer's GitHub profile alongside their PDF resume to generate an objective portfolio scorecard, skill breakdown, and personalized recommendations.

---

## 🚀 Features

- **Resume Parsing**: Synchronous text extraction and skill/language detection from PDF resumes powered by Apache PDFBox with SHA-256 content-hash caching.
- **GitHub Integration**: Connects with GitHub's REST API to gather repository statistics, commit history, language distribution, and star counts.
- **Scoring Engine**: Computes weighted scores across 7 key dimensions:
  - Repository Count & Activity
  - Commit Frequency
  - Language Diversity
  - Project Documentation & Readme Quality
  - Resume vs. GitHub Match
  - Profile Completeness
  - Community Recognition (Stars)
- **Asynchronous Pipeline**: Fast job submission with polling status (`PROCESSING` &rarr; `COMPLETED` / `FAILED`).
- **Persistent Storage**: PostgreSQL database with Spring Data JPA & Hibernate.
- **Interactive UI**: Clean, responsive frontend for resume upload, live analysis progress, scorecard display, and a developer support page.

---

## 🛠️ Prerequisites & Tech Stack

To run this project on any laptop/machine, ensure you have:

| Tool | Version Required | Notes |
| :--- | :--- | :--- |
| **Java JDK** | **JDK 17** or **21+** | Tested on JDK 17, 21, and 25. |
| **PostgreSQL** | **14, 15, or 16+** | Relational DB on port `5432`. |
| **Apache Maven** | **3.8+** *(or use bundled `mvnw`)* | Optional if using `./mvnw` or `mvnw.cmd`. |
| **Docker** *(Optional)* | Any modern version | Run PostgreSQL without manual installation. |

---

## 📦 Quick Start Guide

### 1. Database Setup

#### Option A: Using Local PostgreSQL
1. Start your local PostgreSQL service.
2. Create the database:
   ```sql
   CREATE DATABASE devport;
   ```
3. Update `src/main/resources/application.properties` with your credentials:
   ```properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/devport
   spring.datasource.username=postgres
   spring.datasource.password=YOUR_PASSWORD
   ```

#### Option B: Using Docker Compose
Run the provided Docker Compose file to start PostgreSQL:
```bash
docker compose up -d
```

---

### 2. Configure Environment (Optional but Recommended)

GitHub enforces a rate limit of 60 requests/hour for unauthenticated calls. To avoid rate limits, set a GitHub Personal Access Token:

- **Windows (PowerShell)**:
  ```powershell
  $env:GITHUB_TOKEN="ghp_yourPersonalAccessTokenHere"
  ```
- **Linux / macOS**:
  ```bash
  export GITHUB_TOKEN="ghp_yourPersonalAccessTokenHere"
  ```

---

### 3. Build & Run the Application

#### Windows (PowerShell / CMD)
```powershell
# Using the Maven Wrapper (no separate Maven installation required)
.\mvnw.cmd spring-boot:run

# Or with installed Maven
mvn spring-boot:run
```

#### macOS / Linux
```bash
chmod +x mvnw
./mvnw spring-boot:run
```

---

### 4. Access the Application

Once started, open:
- **Web App**: [http://localhost:8080](http://localhost:8080)
- **Health Check API**: [http://localhost:8080/api/health](http://localhost:8080/api/health)
- **Support Page**: [http://localhost:8080/support.html](http://localhost:8080/support.html)

---

## 📁 Project Structure

```
DevPortfolio-Project-main/
├── src/
│   ├── main/
│   │   ├── java/org/devanalyzer/devport/
│   │   │   ├── config/          # Async, Cache, and GitHub REST configuration
│   │   │   ├── controller/      # REST API endpoints (Analysis & Health)
│   │   │   ├── dto/             # Data Transfer Objects
│   │   │   ├── exception/       # Custom exceptions & handlers
│   │   │   ├── model/           # JPA entities (AnalysisResult)
│   │   │   ├── repository/     # Spring Data JPA repositories
│   │   │   └── service/        # ResumeParser, GitHubService, ScoringEngine
│   │   └── resources/
│   │       ├── static/          # Web frontend (HTML, CSS, assets)
│   │       └── application.properties # Server & DataSource configuration
├── mvnw & mvnw.cmd              # Maven Wrapper scripts
├── pom.xml                      # Project dependencies & build plugins
└── docker-compose.yml           # PostgreSQL container setup
```

---

## 🧪 Testing the API

### Check Service Health
```bash
curl http://localhost:8080/api/health
# Response: {"status":"ok"}
```

### Submit Analysis Job
```bash
curl -X POST http://localhost:8080/api/analyze \
  -F "githubUsername=YOUR_GITHUB_HANDLE" \
  -F "resume=@/path/to/resume.pdf"
```

### Poll Analysis Status
```bash
curl http://localhost:8080/api/analyze/{id}
```

