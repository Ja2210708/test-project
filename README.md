# 🚀 AstroBank Automated Test Suite

A robust, enterprise-grade test automation project designed to validate the core features, REST APIs, and UI behavior of the fictional **AstroBank digital banking platform**. This suite utilizes a hybrid automation framework combining **Java**, **Selenium WebDriver**, **Cucumber**, and **REST Assured**.

---

## 📋 Table of Contents
1. [Project Overview](#-project-overview)
2. [Tech Stack & Prerequisites](#-tech-stack--prerequisites)
3. [Project Structure](#-project-structure)
4. [Environment Configuration](#-environment-configuration)
5. [Getting Started & Installation](#-getting-started--installation)
6. [Running the Tests](#-running-the-tests)
7. [Reporting & Test Output](#-reporting--test-output)
8. [CI/CD Integration](#-cicd-integration)

---

## 🔍 Project Overview

The **AstroBank Test Suite** ensures continuous quality across critical financial workflows. It features a Behavior-Driven Development (BDD) approach to help non-technical stakeholders easily verify acceptance criteria.

### Key Capabilities Verified:
* **UI Testing:** User registration, multi-factor login authentication, dashboard metric rendering, and money transfer validation.
* **API Testing:** End-to-end payload validations for internal microservices (`/api/v1/accounts`, `/api/v1/transactions`).
* **Database Verification:** Post-execution assertions directly against PostgreSQL to confirm data persistence and transactional integrity.

---

## 🛠️ Tech Stack & Prerequisites

Before running the suite, make sure your machine meets the following environment requirements:

* **Programming Language:** Java 21 (JDK 21 or higher)
* **Build Tool:** Apache Maven 3.9+
* **Browsers Required:** Google Chrome (v120+) or Mozilla Firefox
* **Testing Libraries:** 
  * Cucumber JVM (for BDD Gherkin step bindings)
  * JUnit 5 (Test runner engine)
  * REST Assured (API testing layer)
  * WebDriverManager (Automated browser binary engine management)

---

## 📂 Project Structure

```text
astrobank-test-suite/
│
├── src/
│   ├── main/
│   │   └── java/org/astrobank/infra/       # Database helpers, SSH tunnels, configuration loaders
│   └── test/
│       ├── java/org/astrobank/             # Automation implementation core
│       │   ├── runners/                    # Cucumber JUnit runner execution entrypoints
│       │   ├── stepdefs/                   # Gherkin step binding implementations
│       │   ├── pages/                      # Page Object Model (POM) UI layer objects
│       │   └── utils/                      # API client frameworks & custom assertion utilities
│       └── resources/
│           ├── features/                   # Business-facing Gherkin feature files
│           │   ├── ui/                     # Web UI scenarios (Login, Transfer)
│           │   └── api/                    # Microservice contract validation tests
│           └── config/                     # Environment properties files (.env/properties)
├── pom.xml                                 # Maven dependencies and plugin lifecycle profiles
└── README.md                               # Project documentation
```

---

## ⚙️ Environment Configuration

Test parameters dynamically adapt to targeting environments through configuration profiles inside `src/test/resources/config/`.

1. Copy the template configuration file:
   ```bash
   cp src/test/resources/config/env.properties.example src/test/resources/config/env.properties
   ```
2. Populate variables to match your runtime targets:
   ```properties
   astrobank.target.env=staging
   astrobank.ui.url=https://astrobank.internal
   astrobank.api.baseurl=https://astrobank.internal
   browser=chrome
   headless=true
   ```

---

## 🚀 Getting Started & Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com
   cd astrobank-test-suite
   ```

2. **Verify dependencies and compile codebase:**
   ```bash
   mvn clean compile
   ```

3. **Verify driver installation (Dry run):**
   ```bash
   mvn test-compile
   ```

---

## 🧪 Running the Tests

We manage test selection using **Maven Profiles** configured in the `pom.xml`.

### Run All Regression Tests
Executes both UI and API testing paths consecutively:
```bash
mvn clean test -PRegression
```

### Run Specific Test Groups (Using Cucumber Tags)
```bash
# Execute only critical path smoke tests
mvn test -Dcucumber.plugin="pretty" -Dcucumber.features="src/test/resources/features" -Dcucumber.filter.tags="@Smoke"

# Execute only transaction-related API endpoints
mvn test -Dcucumber.filter.tags="@API and @Transactions"
```

### Run Tests in a Specific Browser
Overrides local configurations dynamically:
```bash
mvn test -Dbrowser=firefox -Dheadless=false
```

---

## 📊 Reporting & Test Output

Following execution, tests output structural metrics automatically to the target directory.

### 1. Cucumber Extent Reports
Provides visual HTML dashboards containing screenshots captured automatically upon step failures.
* **Location:** `target/extentreports/Index.html`

### 2. Maven Surefire Execution Logs
Detailed raw log matrix helpful for stack-trace debugging.
* **Location:** `target/surefire-reports/`

---

## 🤖 CI/CD Integration

This test suite is configured for active validation on **GitHub Actions** workflows. Every Pull Request to `main` triggers a headless regression matrix containerized through a standard base runner.

### Sample Pipeline Script Integration:
```yaml
- name: Execute AstroBank Staging Regression
  run: |
    mvn clean test \
    -PRegression \
    -Dastrobank.target.env=staging \
    -Dheadless=true
```
