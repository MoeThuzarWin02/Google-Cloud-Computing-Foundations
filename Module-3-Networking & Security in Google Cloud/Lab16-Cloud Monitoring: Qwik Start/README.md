## Lab 16: Cloud Monitoring: Qwik Start

### Objective
The objective of this lab was to implement comprehensive infrastructure observability and proactive monitoring frameworks on Google Cloud Platform. This was achieved by equipping a Linux-based Compute Engine virtual machine with Google Cloud's consolidated telemetry engine (**Ops Agent**), configuring synthetic availability via automated **Uptime Checks**, setting up threshold-triggered **Alerting Policies** bound to communication channels, constructing a centralized custom performance **Dashboard**, and executing deep-dive diagnostic evaluations using cross-integrated **Cloud Logging** query streams.

### Tools & Services Used
* **Observability Suite:** Cloud Monitoring (Metrics Scope, Uptime Probes, Metric Threshold Alerts) & Cloud Logging (Logs Explorer Query Engine)
* **Compute Infrastructure:** Compute Engine (LAMP Stack Node: `lamp-1-vm` running Debian Linux & Apache2)
* **Telemetry Agents:** Unified Google Cloud Ops Agent daemon (Combining system metric collection and log streaming)
* **Automation Utilities:** Cloud Shell, `gcloud config` utilities, and programmatic Bash installer wrappers

---

### Key Configurations

#### 1. Baseline Workload & Network Footprint
* **Instance Specification:** Name: `lamp-1-vm` | Series: `e2-medium` | OS: Debian Linux.
* **Network Rule:** Permitted standard web access via the native `Allow HTTP traffic` option (opening firewall `tcp:80` for public-facing client calls).
* **Application Core:** Installed Apache2 and configured basic PHP bindings to validate operational serving.

#### 2. Synthetic Availability Probes (Uptime Check)
* **Title:** `Lamp Uptime Check`
* **Protocol & Endpoint:** `HTTP` targeting the direct public `External IP` of the `lamp-1-vm` server.
* **Frequency Intercept:** Exact **1 Minute** ping intervals distributed across multi-region edge testing locations to ensure cross-regional validation.

#### 3. Proactive Alerting Policy Schema
* **Policy Name:** `Inbound Traffic Alert`
* **Target Metric Vector:** `VM Instance > Interface > Network traffic` (`agent.googleapis.com/interface/traffic`)
* **Threshold Condition:** Set to trigger when inbound traffic rises **Above 500 bytes/sec** over a **1-minute** retest window.
* **Notification Channel:** Dedicated **Email Notification Channel** map linked to a display handle for continuous incident logging.

#### 4. Custom Administration Observability Panel
* **Dashboard Identifier:** `Cloud Monitoring LAMP Qwik Start Dashboard`
* **Widget 1 (Line Chart):** System Metric: `VM Instance > Cpu > CPU load (1m)` | Title: `CPU Load`
* **Widget 2 (Line Chart):** System Metric: `VM Instance > Instance > Received packets` | Title: `Received Packets`

---

### Evidence & Results

#### Telemetry Collection Layer Status
Running diagnostic verification commands within the server terminal confirmed the successful installation and active operations of the monitoring system:
```text
$ sudo systemctl status google-cloud-ops-agent
● google-cloud-ops-agent.service - Google Cloud Ops Agent
   Loaded: loaded (/lib/systemd/system/google-cloud-ops-agent.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2026-06-21 16:42:11 UTC; 3min 14s ago
