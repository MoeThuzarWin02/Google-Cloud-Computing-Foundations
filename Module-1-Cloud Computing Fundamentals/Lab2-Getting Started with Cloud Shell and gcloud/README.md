# Lab: Getting Started with Cloud Shell and gcloud

## Summary of the Lab
This lab introduced the fundamentals of interacting with Google Cloud resources using the command-line interface. By leveraging **Cloud Shell**, a managed Debian-based virtual machine, the lab demonstrated how to execute scoped infrastructure actions using the **gcloud CLI** without needing local software installations or cryptographic key management.

## Lab Objective
The objective was to master basic environmental configurations and infrastructure management using `gcloud`. Key milestones included setting global location defaults, provisioning Compute Engine virtual machines, filtering noisy command-line outputs, securely remoting into servers via managed SSH keys, altering active firewall rule matrices, and querying structured project logs.

---

## Key Configurations

### 1. Environment Metadata Properties
* **Global Region & Zone:** Configured using `gcloud config set compute/region <REGION>` and `gcloud config set compute/zone <ZONE>`. These settings ensure that subsequent resource allocation requests default to the designated data center locations.
* **Local Workspace Variables:** Initialized `$PROJECT_ID` and `$ZONE` environment variables to dynamically pass metadata references to programmatic CLI arguments.

### 2. Infrastructure Resources & Networking
* **Compute Instance (`gcelab2`):** Formatted as an `e2-medium` machine type provisioned in the default subnetwork.
* **Network Target Tags:** Appended the tags `http-server` and `https-server` to the VM instance metadata array to identify the host for ingress traffic adjustments.
* **Firewall Ingress Policy (`default-allow-http`):** Created a rule with priority `1000` on the `default` network, permitting incoming TCP traffic over port `80` from any origin (`0.0.0.0/0`) targeted specifically at instances carrying the `http-server` tag.

---

## Evidence

### Command Execution & Connection Verification Matrix
To confirm operational success across the control and data planes, configuration tests were executed directly within the Cloud Shell terminal environment.

**Scenario A: Provisioning and Hardening the Virtual Machine**
After launching the `gcelab2` instance, an internal web server was configured, and firewall rules were updated to make it accessible:
```bash
# Creating the virtual instance via Compute Engine control hooks
$ gcloud compute instances create gcelab2 --machine-type e2-medium --zone $ZONE

# Registering a precise ingress allow rule for web telemetry
$ gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server
