# 🤝 Contributing Guidelines

## Goal

This document outlines **contributor guidelines** for the ROS 2 IoT Adapter project to ensure high-quality, secure, and maintainable contributions aligned with organisational standards.

---

## Table of Contents

1. [Code of Conduct](#code-of-conduct)
2. [Branching Strategy](#branching-strategy)
3. [Commit Message Guidelines](#commit-message-guidelines)
4. [Pull Request (PR) Process](#pull-request-pr-process)
5. [Coding Standards](#coding-standards)
6. [Testing Requirements](#testing-requirements)
7. [Security Considerations](#security-considerations)
8. [Contact](#contact)

---

## Code of Conduct

All contributors must adhere to the [Contributor Covenant Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/).

---

## Branching Strategy

✅ **Main Branch:** Production-ready code only.

✅ **Development Branches:**

* `feature/<name>` for new features.
* `fix/<name>` for bug fixes.
* `chore/<name>` for non-functional updates (e.g. formatting, documentation).

---

## Commit Message Guidelines

Follow the **Conventional Commits** format:

```
<type>(<scope>): <description>

Example:
feat(adapter): add initial support for printer status polling
fix(ci): correct docker build script permissions
```

✅ **Types:** feat, fix, chore, docs, refactor, test

✅ **Scope:** module or area being updated (e.g. adapter, ci, docs)

✅ **Description:** clear, imperative sentence describing the change.

---

## Pull Request (PR) Process

1. Ensure all local tests pass:

```bash
scripts/local_test.sh
scripts/docker_build_test.sh
```

2. Push branch to remote:

```bash
git push origin <branch>
```

3. Create PR:

```bash
gh pr create --fill
```

4. Confirm CI workflow passes for PR validation.

5. Request review from a maintainer.

6. On approval and passing checks, PR will be merged to `main`, triggering CD deployment workflows.

---

## Coding Standards

✅ **Language:** Python 3.10+

✅ **Formatting:** Follow [PEP8](https://peps.python.org/pep-0008/) and enforced via `flake8` or `ruff`.

✅ **Asyncio Design:** Use async-native I/O calls to prevent blocking ROS 2 nodes.

✅ **ROS 2 Standards:**

* Consistent topic naming (`/<device_type>/<device_id>/<topic>`)
* Clear namespace usage for each node

✅ **Docker:**

* Minimal base images (ROS 2 slim variants preferred)
* Non-root user for container execution where possible

---

## Testing Requirements

✅ **Unit Tests:** Required for new adapters and utility modules.

✅ **Integration Tests:** Validate device communication and ROS 2 topic publishing.

✅ **End-to-End Tests:** For critical adapters before production deployment.

---

## Security Considerations

* Never commit credentials, tokens, or secrets.
* Store deployment secrets in **GitHub Actions secrets**.
* Follow container security best practices outlined in [security\_policy.md](docs/security_policy.md).

---

## Contact

For questions, reviews, or design discussions, contact the **project maintainer:**

* Bonny Babu, Application Development Engineer – Robotics & Automation
* Email: <organisation-email-if-applicable>

---

**Thank you for contributing to the ROS 2 IoT Adapter project. Your expertise and diligence enable secure, scalable industrial integrations.**

---

**File path:** `CONTRIBUTING.md`
