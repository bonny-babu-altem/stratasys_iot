# ⚙️ GitHub Actions Workflow Templates

This document provides **production-ready GitHub Actions workflow YAML templates** for CI and CD pipelines in the ROS 2 IoT Adapter project.

---

## 🧪 CI Workflow – `.github/workflows/ci.yml`

```yaml
name: CI

on:
  pull_request:
    branches:
      - main

jobs:
  build-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python 3.10
        uses: actions/setup-python@v5
        with:
          python-version: '3.10'

      - name: Install Python dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Lint with flake8
        run: |
          pip install flake8
          flake8 .

      - name: Run unit tests
        run: |
          chmod +x scripts/local_test.sh
          ./scripts/local_test.sh

      - name: Build and test Docker image
        run: |
          chmod +x scripts/docker_build_test.sh
          ./scripts/docker_build_test.sh
```

---

## 🚀 CD Workflow – `.github/workflows/cd.yml`

```yaml
name: CD

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: [self-hosted, linux, arm64] # Targeting Raspberry Pi runner

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Build Docker image
        run: |
          docker build -t iot-adapter:latest .

      - name: Stop existing container (if running)
        run: |
          docker stop iot-adapter || true
          docker rm iot-adapter || true

      - name: Deploy new container
        run: |
          docker run -d \
            --name iot-adapter \
            --restart always \
            --network host \
            iot-adapter:latest

      - name: Post-deployment verification
        run: |
          docker ps
          docker logs --tail 50 iot-adapter
```

---

## ✅ Notes

* **CI Workflow:** Runs on GitHub-hosted runners, validates PRs with linting, unit tests, and Docker build tests.
* **CD Workflow:** Runs on Raspberry Pi self-hosted runner, builds image, redeploys container, and verifies status.

---

**File path:** `.github/workflows/ci.yml` and `.github/workflows/cd.yml`

➡️ Review, customise environment labels if your Raspberry Pi runner uses specific tags.

---

**End of GitHub Actions workflow templates.**
