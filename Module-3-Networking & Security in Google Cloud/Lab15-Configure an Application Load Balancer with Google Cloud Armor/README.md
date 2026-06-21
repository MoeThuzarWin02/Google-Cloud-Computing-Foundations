## Lab 15: Configure an Application Load Balancer with Google Cloud Armor

### Objective
The objective of this lab was to architect a resilient, highly scalable, and secure edge-routing infrastructure using Google Cloud's Global External Application Load Balancer integrated with Cloud Armor. The design pattern established a cross-regional failover layout using Managed Instance Groups (MIGs) across separate geographic regions, configuring proxy allocation, health check probing intervals, and multi-tier request thresholds. Finally, edge security boundaries were enforced by writing custom Layer 7 Cloud Armor security policies to intercept, mitigate, and denylist malicious high-concurrency traffic over automated logging indicators.

### Tools & Services Used
* **Traffic Management:** Global External Application Load Balancer (HTTP/HTTPS Edge Proxy)
* **Compute Resilience:** Managed Instance Groups (MIGs with Auto-scaling and Custom Instance Templates)
* **Edge Security Perimeters:** Google Cloud Armor (Layer 7 Security Policies & IP Enforcement Denylists)
* **Diagnostic Utilities:** `siege` High-Concurrency Stress Tester, `Cloud Logging` Query Engine, and Advanced Firewall Probes

---

### Key Configurations

#### 1. Ingress Network Protection Perimeters
* **`default-allow-http`**: Permitted `tcp:80` ingress from any external address (`0.0.0.0/0`) targeting instances carrying the `http-server` networking tag.
* **`default-allow-health-check`**: Opened ingress access channels exclusively from the Google Front End (GFE) validation pools (`130.211.0.0/22` and `35.191.0.0/16`) over port `tcp:80` to determine backend vitality.

#### 2. Scaled Backend Topology (MIG Settings)
Both pools were configured with an initialization threshold of 45 seconds and custom Apache metadata initialization via `gs://spls/gsp215/gcpnet/httplb/startup.sh`.

| Instance Group Name | Base Template | Targeting Region | Autoscaling Trigger | Range Boundaries |
| :--- | :--- | :--- | :--- | :--- |
| **`Region 1-mig`** | `Region 1-template` | `Region 1` | CPU Utilization $\ge$ 80% | Min: 1 / Max: 2 Nodes |
| **`Region 2-mig`** | `Region 2-template` | `Region 2` | CPU Utilization $\ge$ 80% | Min: 1 / Max: 2 Nodes |

#### 3. Global Load Balancer Routing Parameters (`http-lb`)
* **Frontend Dual-Stack Binding:** * Port 80 IPv4 Frontend Intercept (Ephemeral Allocation)
  * Port 80 IPv6 Frontend Intercept (Auto-allocated Global Anycast)
* **Backend Services Configuration (`http-backend`):**
  * `Region 1-mig` Balancing Profile: Hard Rate Cap of **50 RPS** per instance at 100% capacity.
  * `Region 2-mig` Balancing Profile: Target Utilization Cap of **80% CPU** load at 100% capacity.
  * Health Probes (`http-health-check`): Interval 5s, Timeout 5s, Healthy/Unhealthy threshold score = 2.

#### 4. Cloud Armor Security Policy Layout
* **Policy Identifier:** `denylist-siege`
* **Default Catch-all Action:** `Allow` (Unmatched client traffic proceeds safely)
* **Rule 1000 (Deny Rule):** Condition matching the specific Client Stress Node IP (`[SIEGE_IP]/32`), responding with a **`403 (Forbidden)`** payload reject signature.

---

### Evidence & Results

#### Multi-User Proximity Routing & Spillover Diagnostics
* **Initial State (Low Traffic):** Initial curl requests targeting `http://[LB_IP_v4]` routed immediately to the closest geographic region (`Region 1-mig`), validating low-latency edge optimization.
* **Stressed State (High Concurrency):** Initiating an aggressive stress footprint from a dedicated third-party zone (`siege-vm`) utilizing 150 concurrent client workers over 120 seconds (`siege -c 150 -t120s http://$LB_IP`) caused the load balancer's Monitoring engine to track cross-regional traffic overflow. Once `Region 1-mig` scaled out and hit its processing threshold limit (50 RPS per node), the load balancer transparently distributed incoming volume out to the remote failover pool (`Region 2-mig`).

#### Cloud Armor Block Verification
* Executing `curl http://$LB_IP` directly from the denylisted `siege-vm` terminal after policy propagation immediately threw a definitive response:
  ```html
  <!doctype html><meta charset="utf-8"><meta name=viewport content="width=device-width, initial-scale=1"><title>403</title>403 Forbidden
