# Lab 6: Google Kubernetes Engine: Qwik Start

## Summary of the Lab
This lab introduced containerized orchestration using **Google Kubernetes Engine (GKE)**, Google Cloud's managed Kubernetes environment. The lab covered the end-to-end lifecycle of a containerized deployment, starting with provisioning a managed cluster infrastructure, configuring the administrative client tool (`kubectl`), deploying a microservice container image, exposing the system to public web traffic via a network load balancer, and tearing down the cluster to optimize cloud resources.

## Lab Objective
The core objective was to understand how GKE automates the scheduling, execution, and external networking of container workloads. Key configurations included utilizing `gcloud` to spin up a three-node pool architecture, injecting an active Kubernetes `Deployment` using an external Container Registry source image, mapping a declarative `LoadBalancer` service to provision a dedicated external IP address, and successfully verifying containerized accessibility over port `8080`.

---

## Key Configurations

### 1. Managed Kubernetes Infrastructure
* **GKE Cluster (`lab-cluster`):** Provisioned as a standard Kubernetes cluster utilizing three `e2-medium` Compute Engine worker nodes distributed inside the default project zone. 
* **Kubernetes Client Context (`kubeconfig`):** Generated cluster access tokens programmatically via `gcloud container clusters get-credentials`, injecting the cluster endpoint context into local Cloud Shell configuration vectors so `kubectl` commands can target the remote control plane.

### 2. Workload and Network Object Manifests
* **Deployment (`hello-server`):** A stateless workload controller configured to run a Google public sample web server image (`gcr.io/google-samples/hello-app:1.0`) on port `8080`.
* **Service Network Proxy (`LoadBalancer`):** Created a network routing object that hooks into Google Cloud's VPC layer to dynamically spin up a hardware Network Load Balancer, providing a single external entry point to forward public traffic down into cluster pods.

---

## Evidence

### GKE Control Plane Initialization and Pod Routing Architecture
To verify cluster lifecycle events and ingress network connectivity, operational tokens were tracked inside the workspace terminal.

**Scenario A: Provisioning the Managed Node Pool**
The control plane setup command initialized three individual virtual machine systems, linking their runtimes into a unified cluster resource block:
```bash
$ gcloud container clusters create --machine-type=e2-medium --zone=$ZONE lab-cluster

Parsed Cluster Topology Output:
NAME         LOCATION       MASTER_VERSION    MASTER_IP      MACHINE_TYPE  NUM_NODES  STATUS
lab-cluster  us-central1-a  1.22.8-gke.202    34.67.240.12   e2-medium     3          RUNNING
