# 🛠️ ROS 2 IoT Adapter

## Purpose

This repository contains the **ROS 2 IoT Adapter framework**, enabling integration of 3D printers, robots, and industrial devices into an IoT platform with scalable, maintainable, and secure architecture.

---

## Architectural Context

* **Framework:** ROS 2 Jazzy
* **Language:** Python (asyncio)
* **Development OS:** Ubuntu 24.04 (WSL2)
* **Deployment OS:** Ubuntu 24.04 (Raspberry Pi)
* **Deployment Mode:** Docker containers managed via GitHub Actions CD workflows
* **CI/CD:** GitHub Actions with Raspberry Pi as self-hosted runner
* **Key Design Elements:**

  * Modular adapter classes per device model
  * Concurrent multi-device support using ROS 2 nodes
  * Async client design for efficient I/O operations

---

## Prerequisites

✅ **Development Environment**

* [ ] Ubuntu 24.04 (WSL2 or native)
* [ ] Python >= 3.10 with venv support
* [ ] ROS 2 Jazzy installed
* [ ] Docker and Docker Compose installed
* [ ] VS Code with Remote - Containers extension
* [ ] GitHub CLI (optional for PR workflows)

✅ **Deployment Environment**

* [ ] Raspberry Pi 4 with Ubuntu 24.04
* [ ] Docker installed and running
* [ ] Self-hosted GitHub Actions runner registered and active

---

## Setup Instructions

### 1. Clone the Repository

```bash
git clone [https://github.com/your-org/ros2-iot-adapter.git](https://github.com/bonny-babu-altem/altem_iot.git)
cd ros2-iot-adapter
```

### 2. Create and Activate Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
pip install -r dev-requirements.txt
```

### 4. Build Docker Image

```bash
docker build -t iot-adapter:latest .
```

---

## Usage Instructions

### Run Local Unit Tests

```bash
scripts/local_test.sh
```

### Launch ROS 2 Nodes (Example)

```bash
ros2 launch iot_adapter main_launch.py
```

### Run as Docker Container

```bash
docker run -d \
  --name iot-adapter \
  --restart always \
  --network host \
  iot-adapter:latest
```

### CI/CD Workflow

* **CI:** Runs on pull requests to `main`, performing linting, unit tests, and Docker build validation.
* **CD:** On merge to `main`, deploys container to Raspberry Pi via GitHub Actions self-hosted runner.

---

## TODO Checklist

* [ ] Develop adapter classes for remaining device models
* [ ] Implement end-to-end integration tests
* [ ] Optimise Docker images for production deployment
* [ ] Finalise ROS 2 QoS and security configurations
* [ ] Document device onboarding flows in `/docs`
* [ ] Implement automated rollback in deployment workflows

---

## Troubleshooting

| Issue                      | Possible Cause                          | Resolution                                             |
| -------------------------- | --------------------------------------- | ------------------------------------------------------ |
| ROS 2 launch fails         | ROS environment not sourced             | Source `/opt/ros/jazzy/setup.bash` before running      |
| Docker build error         | Outdated or incompatible base image     | Pull latest ROS 2 image and rebuild                    |
| Self-hosted runner offline | Runner service stopped or network issue | Restart runner on Raspberry Pi and verify connectivity |
| Adapter connection timeout | Incorrect device IP or configuration    | Check device configuration and network reachability    |
| CI workflow fails          | Missing dependencies or setup commands  | Review workflows and requirements files                |

---

## Summary

This repository delivers a production-grade ROS 2 IoT Adapter framework for integrating industrial devices into IoT platforms with a focus on clean architecture, asynchronous design, and automated CI/CD.

➡️ **Next:** Refer to [docs/architecture\_design\_decisions.md](docs/architecture_design_decisions.md) for detailed design justifications and alternatives considered.

---
