# Lab 3: Create a Virtual Machine

## Summary of the Lab
This lab focused on provisioning and managing Infrastructure-as-a-Service (IaaS) compute instances on Google Cloud Platform. It demonstrated the operational dual-track of cloud administration: spinning up virtual machines (VMs) via the visual, click-driven Google Cloud Console web UI and executing identical deployments programmatically using the `gcloud` command-line interface in Cloud Shell.

## Lab Objective
The objective was to deploy, configure, and expose web server workloads on **Compute Engine** virtual instances. The engineering core involved establishing geographical resource isolation (Regions/Zones), initializing independent Linux runtimes, deploying an Nginx web server engine, and altering underlying VPC firewall routes to expose internal application ports safely to public traffic.

---

## Key Configurations

### 1. Virtual Machine (VM) Topology
* **Console Instance (`gcelab`):** Formatted as an `e2-medium` machine family runner, provisioned with a 10 GB balanced persistent boot disk running a Debian Linux image distribution. 
* **CLI Instance (`gcelab2`):** Deployed programmatically via the SDK core using runtime environment flags matching the identical `e2-medium` machine type parameters.

### 2. VPC Networking & Perimeter Security
* **Network Target Tags:** Enabled via the **"Allow HTTP traffic"** console routing flag. This configuration dynamically binds the `http-server` network metadata tag directly to the host instance's VNIC array.
* **Implicit Ingress Firewall Rules:** The platform utilizes the network tag assignment to implicitly target the instance with an automated VPC edge policy, opening up ingress `TCP:80` data pathways from all source internet ranges (`0.0.0.0/0`).

---

## Evidence

### VM Provisioning & Data Plane Reachability Telemetry
To verify structural execution, instances were instantiated using both control pathways, and internal application states were queried to track runtime status.

**Scenario A: Programmatic Instance Generation via CLI Engine**
The instance properties were injected through Cloud Shell to construct the second operational node without using the graphical web console interface:
```bash
$ gcloud compute instances create gcelab2 --machine-type e2-medium --zone=$ZONE

Parsed Infrastructure Log:
Created [[https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-00-gsp001/zones/us-central1-a/instances/gcelab2](https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-00-gsp001/zones/us-central1-a/instances/gcelab2)].
NAME     ZONE           MACHINE_TYPE  PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP    STATUS
gcelab2  us-central1-a  e2-medium     —            10.128.0.3   34.136.51.150  RUNNING
