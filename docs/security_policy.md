# 🔒 Security Policy

## Goal

This document defines the **security policies and best practices** for the ROS 2 IoT Adapter project to ensure safe development, deployment, and operation within industrial and IoT environments.

---

## Scope

* Development systems (WSL2, local Ubuntu)
* GitHub repository and Actions workflows
* Docker container builds and runtime
* Deployment on Raspberry Pi devices
* ROS 2 communication between nodes and services

---

## Security Objectives

1. **Confidentiality:** Protect sensitive credentials and device configurations.
2. **Integrity:** Ensure code and container images are tamper-proof.
3. **Availability:** Maintain continuous operation with minimal security risks.
4. **Compliance:** Follow container, DevOps, and ROS 2 security best practices.

---

## Authentication and Authorisation

✅ **GitHub Repository**

* Enforce **2FA** for all contributors.
* Use **branch protection rules** to require PR reviews and passing CI before merge.
* Store sensitive variables (e.g. device passwords, tokens) in **GitHub Actions secrets**.

✅ **Raspberry Pi Deployment**

* Limit SSH access to trusted personnel with strong keys.
* Regularly rotate SSH keys.
* Run containers as **non-root users** where possible.

---

## Network Security

✅ **ROS 2 Communication**

* Keep ROS 2 traffic within trusted local networks.
* Use **DDS Security plugins** for authenticated and encrypted communication if operating across untrusted networks.

✅ **Device Connections**

* Ensure device IPs and ports are configured securely.
* Avoid exposing device interfaces to public networks.

✅ **Container Network Configurations**

* Use `--network host` only when required for device communication.
* Apply internal network segregation if deploying multiple services with interdependencies.

---

## Container Security

✅ **Docker Images**

* Use minimal base images (e.g. ROS 2 slim variants).
* Implement **multi-stage builds** to reduce final image size and attack surface.
* Scan images for vulnerabilities using tools such as `docker scan` or GitHub Advanced Security.

✅ **Runtime**

* Avoid running containers with root user unless strictly necessary.
* Restrict container capabilities as per least privilege principle.

---

## GitHub Actions Security

✅ **Secrets Management**

* Store deployment and environment secrets in **GitHub Actions secrets** only.
* Do not hardcode credentials or tokens in workflows or code.

✅ **Self-hosted Runner Security**

* Run the GitHub Actions runner as a **dedicated low-privilege user**.
* Keep the Raspberry Pi OS updated with security patches.
* Monitor runner activity for unexpected jobs or privilege escalations.

---

## ROS 2 Communication Security

✅ **DDS Security**

* Evaluate and implement DDS security features for authentication, encryption, and access control if deployment extends beyond trusted internal networks.
* Maintain ROS 2 node and topic namespace hygiene to prevent unintended data exposure.

---

## TODO Checklist

* [ ] Implement DDS Security plugin testing in staging environment.
* [ ] Enforce non-root user for all containerised services.
* [ ] Integrate automated container vulnerability scanning in CI workflow.
* [ ] Document secret rotation procedures for GitHub Actions and deployment environments.
* [ ] Review ROS 2 QoS settings for potential denial-of-service attack surfaces.

---

## Troubleshooting

| Issue                            | Possible Cause                | Resolution                                                                   |
| -------------------------------- | ----------------------------- | ---------------------------------------------------------------------------- |
| Deployment secrets exposed       | Secrets committed to repo     | Remove committed secrets, rotate immediately, and use GitHub Actions secrets |
| Unauthorised SSH access attempts | Weak or default passwords     | Disable password login, enforce key-based authentication                     |
| ROS 2 node communication fails   | DDS security misconfiguration | Verify security XML profiles and DDS plugin settings                         |
| Container cannot access device   | Dropped network capabilities  | Adjust container runtime permissions and network mode                        |

---

## Summary

This document establishes the **security policies** required for safe development and deployment of the ROS 2 IoT Adapter framework in production environments.

➡️ **Next:** Refer to [system\_integration\_guide.md](system_integration_guide.md) for end-to-end integration flow and validation strategy.

---
