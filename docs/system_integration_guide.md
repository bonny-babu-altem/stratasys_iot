# 🔗 System Integration Guide

## Goal

This document provides the **end-to-end integration guide** for the ROS 2 IoT Adapter framework, detailing interface definitions, data contracts, and validation strategy for industrial deployment.

---

## Architectural Context

* **Project:** ROS 2 IoT Adapter
* **Language:** Python (asyncio)
* **Framework:** ROS 2 Jazzy
* **Deployment:** Docker containers on Raspberry Pi (Ubuntu 24.04)
* **Integration Target:** IoT Platform via MQTT or internal ROS 2 data processing pipelines

---

## Integration Flow

1. **Device Adapter Initialisation:**

   * Each adapter class is instantiated per device model with connection parameters.
   * Async clients establish communication with device endpoints.

2. **ROS 2 Node Spin:**

   * Adapter runs as a ROS 2 node with dedicated namespace and lifecycle.

3. **Data Acquisition:**

   * Device data is polled or subscribed (depending on protocol) asynchronously.

4. **Data Publishing:**

   * Parsed device data is published to ROS 2 topics with appropriate QoS settings.

5. **IoT Platform Integration:**

   * ROS 2 topic bridges or dedicated MQTT publisher nodes forward data to the IoT platform.

---

## Interface Definitions

### Device Adapter Interface

| Method         | Description                                     |
| -------------- | ----------------------------------------------- |
| `connect()`    | Establishes async connection to device endpoint |
| `disconnect()` | Graceful teardown of device connection          |
| `read()`       | Reads and parses device data payload            |
| `publish()`    | Publishes parsed data to ROS 2 topic            |

### ROS 2 Topic Contracts

Example: **3D Printer Status Topic**

| Field          | Type     | Description                           |
| -------------- | -------- | ------------------------------------- |
| `printer_id`   | `string` | Unique identifier of printer          |
| `status`       | `string` | Current operational status            |
| `job_progress` | `float`  | Current print job completion %        |
| `temperature`  | `float`  | Current nozzle or chamber temperature |

---

## Data Contracts

All published data structures must adhere to ROS 2 message definitions documented under `/msg` in each adapter package.

✅ **Standardisation Guidelines:**

* Use consistent units (e.g. Celsius for temperature, mm for dimensions).
* Include timestamps with all published data.
* Maintain schema versioning in message definitions for future compatibility.

---

## Testing Strategy

✅ **Unit Tests**

* Validate parsing logic for device payloads.
* Test adapter methods with simulated device responses.

✅ **Integration Tests**

* Deploy containers in staging environment.
* Use mock devices or simulators to emulate data streams.
* Verify ROS 2 topic publications match defined data contracts.

✅ **End-to-End Validation**

* Run system with real devices in controlled environment.
* Confirm data appears in IoT platform with correct field mapping and value integrity.
* Perform network disruption and device restart tests to validate adapter resilience.

---

## TODO Checklist

* [ ] Finalise data contracts for all device types
* [ ] Implement automated integration tests for each adapter
* [ ] Document topic-to-IoT field mapping for all integrations
* [ ] Develop device simulators for offline integration testing
* [ ] Review ROS 2 QoS policies for reliability and performance

---

## Troubleshooting

| Issue                                | Possible Cause                                     | Resolution                                            |
| ------------------------------------ | -------------------------------------------------- | ----------------------------------------------------- |
| No data published on ROS 2 topic     | Adapter not connected or failed silently           | Check container logs and device network reachability  |
| Data schema mismatch in IoT platform | Topic mapping misconfigured                        | Review mapping definitions and revalidate fields      |
| Adapter crashes on device disconnect | Unhandled exception in read loop                   | Implement robust exception handling in adapter client |
| Delayed data updates                 | Inefficient polling intervals or QoS configuration | Optimise polling frequency and ROS 2 QoS settings     |

---

## Summary

This integration guide ensures consistent, validated, and production-ready integration of the ROS 2 IoT Adapter framework with device endpoints and IoT platforms.

➡️ **Next:** Refer to [ci\_cd\_pipeline.md](ci_cd_pipeline.md) for continuous integration and deployment workflows supporting this system integration.

---