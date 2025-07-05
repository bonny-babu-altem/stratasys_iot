# 📚 Project Documentation Summary

## ROS 2 IoT Adapter

This **SUMMARY.md** provides an index of all documentation files within the ROS 2 IoT Adapter project, enabling efficient navigation, onboarding, and implementation alignment.

---

### 🏠 Main Project

* [README.md](../README.md) – Project overview, purpose, setup, and usage instructions.

---

### 📂 Docs

| Document                                                               | Description                                                                                    |
| ---------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| [architecture\_design\_decisions.md](architecture_design_decisions.md) | High-level and detailed architecture decisions, alternatives considered, and justifications.   |
| [deployment\_playbook.md](deployment_playbook.md)                      | Step-by-step deployment procedures, rollback strategy, and verification guidelines.            |
| [security\_policy.md](security_policy.md)                              | Security objectives, policies, and best practices for development, deployment, and operations. |
| [system\_integration\_guide.md](system_integration_guide.md)           | End-to-end integration flow, interface definitions, data contracts, and testing strategy.      |
| [ci\_cd\_pipeline.md](ci_cd_pipeline.md)                               | CI/CD pipeline setup, local scripts, GitHub Actions workflows, and troubleshooting.            |

---

### 📂 Scripts

* **scripts/local\_test.sh** – Executes local unit tests and linting.
* **scripts/docker\_build\_test.sh** – Builds and tests Docker images locally.

---

### ⚙️ GitHub Workflows

| Workflow                                                | Description                                                                 |
| ------------------------------------------------------- | --------------------------------------------------------------------------- |
| [.github/workflows/ci.yml](../.github/workflows/ci.yml) | CI workflow for PR validation, linting, unit tests, and Docker build tests. |
| [.github/workflows/cd.yml](../.github/workflows/cd.yml) | CD workflow for automated deployment to Raspberry Pi self-hosted runner.    |

---

## ✅ Next Steps

➡️ **Review** each document for project alignment and implementation.

➡️ **Update** any TODO checklists to maintain delivery velocity.

➡️ **Proceed** to [CONTRIBUTING.md](CONTRIBUTING.md) for contributor guidelines and development standards.

---