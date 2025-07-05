# ⚙️ CI/CD Pipeline Setup

## Goal

This document defines the **Continuous Integration and Continuous Deployment (CI/CD) pipelines** for the ROS 2 IoT Adapter project, enabling automated testing, building, and deployment to production Raspberry Pi environments.

---

## Architectural Context

* **CI/CD Platform:** GitHub Actions
* **Self-hosted Runner:** Raspberry Pi Ubuntu 24.04 for CD workflows
* **Containerisation:** Docker build and deployment for consistent environments
* **Key Pipelines:**

  * Local development scripts
  * GitHub Actions CI workflow (PR validation)
  * GitHub Actions CD workflow (deployment)

---

## Prerequisites

✅ **Repository Configuration**

* [ ] GitHub Actions enabled
* [ ] Self-hosted runner registered for deployment jobs
* [ ] Secrets configured for any credentials or environment variables

✅ **Development System**

* [ ] Docker installed and running
* [ ] ROS 2 Jazzy installed for local tests
* [ ] Executable permissions for scripts

---

## Local Test Scripts

### 1. Unit Tests

```bash
scripts/local_test.sh
```

✅ Runs linting and Python unit tests to validate core logic.

### 2. Docker Build Test

```bash
scripts/docker_build_test.sh
```

✅ Builds the Docker image locally to validate Dockerfile and dependencies.

---

## Docker Build and Test Scripts

### scripts/docker\_build\_test.sh

```bash
#!/usr/bin/env bash
set -e

echo "Building ROS 2 IoT Adapter Docker image..."
docker build -t iot-adapter:latest .

echo "Running container for validation..."
docker run --rm \
  --network host \
  iot-adapter:latest

echo "Docker build and run test completed successfully."
```

---

## Pull Request Workflow (CI)

✅ **.github/workflows/ci.yml**

### Key Steps

1. Checkout repository
2. Set up Python and ROS 2 environment
3. Install dependencies
4. Run linting (`flake8` or `ruff`)
5. Run unit tests
6. Build Docker image for validation

✅ **Trigger:** On pull requests targeting `main`

---

## Merge Workflow (CD)

✅ **.github/workflows/cd.yml**

### Key Steps

1. Checkout repository on Raspberry Pi self-hosted runner
2. Build Docker image
3. Stop and remove existing container (if any)
4. Run new container with `--restart always`
5. Notify deployment success or failure

✅ **Trigger:** On push to `main` branch

---

## PR Creation and Merge Flow

1. **Feature Development:** Create feature branch locally.

```bash
git checkout -b feature/<name>
```

2. **Commit Changes:**

```bash
git add .
git commit -m "Feature: <name>"
```

3. **Push and Raise PR:**

```bash
git push origin feature/<name>
gh pr create --fill
```

4. **CI Validation:** PR triggers CI workflow.

5. **Review and Merge:** On approval and passing CI, merge to `main` triggers CD workflow.

---

## TODO Checklist

* [ ] Integrate container vulnerability scanning in CI workflow
* [ ] Implement automated rollback on CD failure
* [ ] Add Slack or Teams notifications for deployment status
* [ ] Optimise Docker build cache strategy in workflows
* [ ] Document PR approval policies in CONTRIBUTING.md

---

## Troubleshooting

| Issue                            | Possible Cause                               | Resolution                                        |
| -------------------------------- | -------------------------------------------- | ------------------------------------------------- |
| CI fails on ROS 2 setup          | Missing ROS 2 dependencies in workflow       | Update CI workflow with ROS 2 Jazzy setup steps   |
| CD workflow fails                | Self-hosted runner offline                   | Restart runner service on Raspberry Pi            |
| Docker build fails               | Base image update incompatibility            | Pin ROS 2 base image tags or rebuild dependencies |
| Deployment container not running | Entrypoint error or missing environment vars | Check container logs for detailed errors          |

---

## Summary

This document defines the CI/CD pipeline setup for **automated testing, building, and deployment** of the ROS 2 IoT Adapter framework, ensuring production-grade delivery workflows.

➡️ **Next:** Review [architecture\_design\_decisions.md](architecture_design_decisions.md) to align CI/CD implementation with system design principles.

---