# 🚀 Deployment Playbook

## Goal

This playbook defines the **deployment procedures** for the ROS 2 IoT Adapter project, ensuring reproducible, secure, and production-grade rollout to Raspberry Pi devices running Ubuntu 24.04.

---

## Architectural and Deployment Context

* **Target Device:** Raspberry Pi 4
* **OS:** Ubuntu 24.04
* **Deployment Mode:** Docker containerised services
* **CI/CD Pipeline:** GitHub Actions with Raspberry Pi self-hosted runner for CD workflows
* **Service Design:** Each adapter runs as an independent containerised ROS 2 node with network host mode for device communication.

---

## Prerequisites

✅ **Raspberry Pi Environment**

* [ ] Raspberry Pi 4 with Ubuntu 24.04 installed
* [ ] User added to `docker` group
* [ ] Docker installed and running
* [ ] Self-hosted GitHub Actions runner registered and active
* [ ] SSH access configured for maintenance operations

✅ **GitHub Repository**

* [ ] Actions workflows configured for CD deployment
* [ ] Secrets configured for deployment (if environment variables required)

---

## Step-by-Step Deployment Instructions

### 1. Build Docker Image Locally (Optional Validation)

```bash
git clone https://github.com/your-org/ros2-iot-adapter.git
cd ros2-iot-adapter

# Build the image
sudo docker build -t iot-adapter:latest .

# Test container locally before remote deployment
sudo docker run --rm \
  --network host \
  iot-adapter:latest
```

### 2. Push to Main Branch to Trigger CD Workflow

Commit and push your changes:

```bash
git add .
git commit -m "Production release: <feature/fix>"
git push origin main
```

The **GitHub Actions CD workflow** will:

1. Checkout code on Raspberry Pi self-hosted runner.
2. Build the Docker image.
3. Stop and remove any existing container.
4. Deploy the new container with `--restart always` for service resilience.

### 3. Manual Deployment Commands (If Needed)

#### Stop Existing Container

```bash
sudo docker stop iot-adapter || true
sudo docker rm iot-adapter || true
```

#### Run New Container

```bash
sudo docker run -d \
  --name iot-adapter \
  --restart always \
  --network host \
  iot-adapter:latest
```

---

## Rollback Strategy

1. Identify the last known stable image tag using:

```bash
sudo docker images
```

2. Stop and remove the faulty container:

```bash
sudo docker stop iot-adapter
sudo docker rm iot-adapter
```

3. Run the previous stable image:

```bash
sudo docker run -d \
  --name iot-adapter \
  --restart always \
  --network host \
  iot-adapter:<previous-stable-tag>
```

---

## Verification Steps

✅ **Container Status Check**

```bash
sudo docker ps
```

✅ **Container Logs Review**

```bash
sudo docker logs -f iot-adapter
```

✅ **ROS 2 Node Verification**

Inside container or from host ROS 2 environment:

```bash
ros2 node list
```

✅ **Device Communication Check**

Ensure each adapter can successfully connect to its device endpoint and publish data to the expected ROS 2 topics.

---

## Troubleshooting

| Issue                       | Possible Cause                                      | Resolution                                         |
| --------------------------- | --------------------------------------------------- | -------------------------------------------------- |
| Deployment workflow fails   | Runner offline or Docker daemon error               | Restart runner service and Docker on Raspberry Pi  |
| Container exits immediately | Entrypoint misconfiguration                         | Check Dockerfile CMD and logs for errors           |
| Device connection refused   | Network misconfiguration                            | Verify IP, port, and physical network connectivity |
| Old container still running | Deployment script did not remove existing container | Manually stop and remove using commands above      |

---

## Security Best Practices

* Run containers with minimal privileges and avoid root processes when possible.
* Use **GitHub Actions secrets** for any deployment credentials or environment variables.
* Regularly update Docker base images to include security patches.
* Restrict SSH access to Raspberry Pi deployment hosts to trusted personnel only.
* Apply ROS 2 DDS security plugins if network communication is outside trusted boundaries.

---

## Summary

This playbook defines deployment, rollback, and verification procedures for the ROS 2 IoT Adapter project on Raspberry Pi targets, ensuring reproducibility and operational resilience.

➡️ **Next:** Refer to [security\_policy.md](security_policy.md) for deployment and operational security guidelines.

---