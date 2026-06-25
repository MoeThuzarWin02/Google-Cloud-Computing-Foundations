# Lab 1: A Tour of Google Cloud Hands-on Labs

## Summary of the Lab
This lab served as a foundational orientation to the Google Cloud hands-on learning environment. It outlined the operational mechanics of the orchestration platform, demonstrating how temporary sandbox credentials interface securely with the browser-based Google Cloud Console without conflicting with personal or corporate accounts.

## Lab Objective
The objective was to familiarize myself with the core navigational hubs and structures of Google Cloud Platform (GCP). The lab covered managing resource organization layers (Projects), identifying default primitive permissions, modifying identity access controls using Cloud IAM, and interacting with the managed cloud marketplace to discover and programmatically enable Google APIs.

---

## Key Configurations

### 1. Identity & Access Management (IAM)
* **`roles/editor` (Project Editor):** Automatically mapped to the primary temporary lab account (`student-xx-xxxxxx@qwiklabs.net`). This grants active state-mutation authority over compute, network, and storage buckets while preventing modifications to global project-member lists.
* **`roles/viewer` (Project Viewer):** Programmatically assigned to a secondary principal placeholder (`User 2`) inside the IAM console dashboard to practice policy updates and enforce read-only resource inspection boundaries.

### 2. API Services Architecture
* **Dialogflow API:** Selected, inspected, and programmatically initialized within the project space. Enabling this framework unlocks conversational natural language schema endpoints, linking managed machine learning capabilities to our active project namespace.

---

## Evidence

### IAM Policy Modification & API Activation Matrix
To verify complete engagement with the cloud plane, administrative tasks were performed within the resource framework to track project status updates.

**Scenario A: Administering Identity Modifications (Cloud IAM)**
The interface console was accessed to append a runtime policy change, successfully registering a new identity block under the project schema tree:
```text
IAM Console Action: Grant Access
Principal Added: User 2
Assigned Role: roles/viewer
State: Policy verified and applied via cloud resource manager.

## Wiki Link
https://github.com/MoeThuzarWin02/Google-Cloud-Computing-Foundations/wiki/Lab-1:-A-Tour-of-Google-Cloud-Hands%E2%80%90on-Labs
