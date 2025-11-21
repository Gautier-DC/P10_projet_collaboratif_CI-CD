# CI/CD Documentation - BobApp

## Overview

This document describes the CI/CD pipeline implemented for the BobApp project, an application composed of a Java backend (Spring Boot) and an Angular frontend.

---

## 1. Workflow Steps

The CI/CD pipeline is defined in the `.github/workflows/ci-cd.yml` file and runs automatically on each push to the `main` branch or on pull requests.

### 1.1 Code Checkout
**Objective**: Retrieve the source code from the repository with full history (required for SonarCloud analysis).

### 1.2 Backend Build and Tests

| Step | Objective |
|------|-----------|
| **Set up JDK 11** | Configure Java environment with Eclipse Temurin JDK 11 |
| **Build Backend** | Compile Java code and generate JAR file with Maven (`mvn clean install`) |
| **Run Backend Tests with Coverage** | Execute unit tests and generate JaCoCo coverage report |

### 1.3 Frontend Build and Tests

| Step | Objective |
|------|-----------|
| **Set up Node.js** | Configure Node.js environment version 16 |
| **Install Frontend Dependencies** | Install npm dependencies for the Angular project |
| **Build Frontend** | Compile the Angular application for production |
| **Run Frontend Tests** | Execute Karma/Jasmine tests with LCOV coverage report generation |

### 1.4 Code Quality Analysis

| Step | Objective |
|------|-----------|
| **SonarCloud Scan Backend** | Analyze Java code quality (bugs, vulnerabilities, code smells, coverage) |
| **SonarCloud Scan Frontend** | Analyze TypeScript/Angular code quality (bugs, vulnerabilities, code smells, coverage) |

### 1.5 Docker Image Publication

| Step | Objective |
|------|-----------|
| **Login to Docker Hub** | Authenticate with Docker Hub |
| **Build and Push Backend Docker Image** | Build and publish the backend Docker image to Docker Hub |
| **Build and Push Frontend Docker Image** | Build and publish the frontend Docker image to Docker Hub |

**Note**: Docker publication only occurs on pushes to `main` (not on pull requests).

---

## 2. KPIs (Key Performance Indicators)

To ensure code quality and application stability, the following KPIs are defined:

### KPI 1: Minimum Code Coverage

| Metric | Minimum Threshold | Justification |
|--------|-------------------|---------------|
| **Backend Code Coverage** | **80%** | Ensures most Java code is tested, reducing regression risks |
| **Frontend Code Coverage** | **80%** | Guarantees reliability of Angular components and services |

### KPI 2: Zero Blocker and Critical Bugs

| Metric | Threshold | Justification |
|--------|-----------|---------------|
| **Bugs (Blocker/Critical)** | **0** | No blocker or critical bugs should be present in deployed code |

### KPI 3: Reliability Rating

| Metric | Minimum Threshold | Justification |
|--------|-------------------|---------------|
| **Reliability Rating** | **B or better** | Code must maintain an acceptable reliability level (A being optimal) |

### KPI 4: No Security Vulnerabilities

| Metric | Threshold | Justification |
|--------|-----------|---------------|
| **Vulnerabilities** | **0** | No known security vulnerabilities should be present |

---

## 3. Current Metrics Analysis

### 3.1 Backend Metrics

| Metric | Current Value | Status | KPI |
|--------|---------------|--------|-----|
| Security | - | - | - |
| Reliability | **D** (1 bug) | **NON-COMPLIANT** | Must be B or better |
| Maintainability | **A** (6 code smells) | Compliant | - |
| Hotspots Reviewed | **E** (0.0%) | **NEEDS IMPROVEMENT** | Security hotspots not reviewed |
| Coverage | **38.8%** | **NON-COMPLIANT** | Target: 80% |
| Duplications | **0.0%** | Compliant | - |

### 3.2 Frontend Metrics

| Metric | Current Value | Status | KPI |
|--------|---------------|--------|-----|
| Security | **A** (0) | Compliant | - |
| Reliability | **A** (0 bugs) | Compliant | - |
| Maintainability | **A** (6 code smells) | Compliant | - |
| Hotspots Reviewed | **A** (100%) | Compliant | - |
| Coverage | **47.6%** | **NON-COMPLIANT** | Target: 80% |
| Duplications | **0.0%** | Compliant | - |

### 3.3 Summary

| Project | Strengths | Areas for Improvement |
|---------|-----------|----------------------|
| **Backend** | No duplication, good maintainability | Insufficient coverage (38.8%), 1 bug to fix, hotspots not reviewed |
| **Frontend** | Good security, reliability, hotspots reviewed | Insufficient coverage (47.6%) |

---

## 4. Recommendations

### 4.1 High Priority - Immediate Actions

#### Fix the backend bug (Reliability D)
- **Action**: Identify and fix the bug detected by SonarCloud
- **Impact**: Improve backend reliability from D to A/B
- **Timeline**: Immediate

#### Increase backend coverage (38.8% → 80%)
- **Action**: Add unit tests for uncovered classes
- **Impact**: Reduce regression risks, achieve 80% KPI
- **Timeline**: Current sprint

### 4.2 Medium Priority

#### Increase frontend coverage (47.6% → 80%)
- **Action**: Complete tests for Angular components and services
- **Impact**: Achieve 80% coverage KPI
- **Timeline**: Next sprint

#### Review backend Security Hotspots (0%)
- **Action**: Analyze and validate security hotspots identified by SonarCloud
- **Impact**: Improve security rating
- **Timeline**: Next sprint

### 4.3 Low Priority - Continuous Improvement

#### Reduce Code Smells
- **Action**: Refactor the 6 code smells identified in each project
- **Impact**: Improve long-term maintainability
- **Timeline**: Backlog

### 4.4 SonarCloud Quality Gate Configuration

It is recommended to configure a **Quality Gate** in SonarCloud with the following conditions:

```
- Coverage < 80% → Fail
- Bugs > 0 (Blocker/Critical) → Fail
- Vulnerabilities > 0 → Fail
- Security Hotspots Reviewed < 100% → Fail
```

This will automatically block merges that do not meet the defined KPIs.

---

## 5. Tool Access

| Tool | URL | Description |
|------|-----|-------------|
| SonarCloud Backend | [Link](https://sonarcloud.io/project/overview?id=Gautier-DC_P10_backend) | Backend quality dashboard |
| SonarCloud Frontend | [Link](https://sonarcloud.io/project/overview?id=Gautier-DC_P10_frontend) | Frontend quality dashboard |
| Docker Hub Backend | [Link](https://hub.docker.com/r/gautierdc/bobapp-backend) | Backend Docker image |
| Docker Hub Frontend | [Link](https://hub.docker.com/r/gautierdc/bobapp-frontend) | Frontend Docker image |
| GitHub Actions | [Link](https://github.com/Gautier-DC/P10_projet_collaboratif_CI-CD/actions) | Workflow history |

---

## 6. Required GitHub Secrets

For the pipeline to work correctly, the following secrets must be configured in GitHub:

| Secret | Description |
|--------|-------------|
| `SONAR_TOKEN` | SonarCloud authentication token |
| `DOCKER_USERNAME` | Docker Hub username |
| `DOCKER_TOKEN` | Docker Hub access token |

---

## 7. Conclusion

The implemented CI/CD pipeline fully automates the build, test, analysis, and deployment process for the BobApp application.

**Positive points**:
- Complete and automated pipeline
- Clear separation of backend/frontend analyses
- Automatic Docker image publication
- Good code maintainability (A rating)

**Priority improvement areas**:
1. Fix the backend bug to improve reliability
2. Increase code coverage for both projects to 80%
3. Review backend security hotspots

Adhering to the defined KPIs will ensure the quality and stability of the application in production.
