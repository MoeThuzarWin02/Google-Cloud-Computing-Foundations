# Lab 12: Cloud IAM: Qwik Start

## Summary of the Lab
This lab provided a hands-on exploration of Google Cloud's Identity and Access Management (IAM) framework by simulating a real-world multi-user environment. Using two distinct sets of credentials, the lab demonstrated how primitive project-level roles and fine-grained, resource-specific roles alter a user's ability to view, create, or modify cloud infrastructure elements.

## Lab Objective
The objective was to understand the practical applications of the **Principle of Least Privilege (PoLP)** within Google Cloud. By actively granting, checking, revoking, and shifting permissions between a Project Owner (User 1) and a restricted user (User 2), the lab illustrated how IAM policies propagate across resources like **Cloud Storage Buckets** and restrict unauthorized Console/CLI API actions.

---

## Key Configurations

### 1. Identity & Access Management (IAM) Roles
* **`roles/owner` (Project Owner):** Pre-assigned to User 1. This primitive role granted complete administrative control, including the critical `resource manager.projects.setIamPolicy` permission needed to mutate project access lists.
* **`roles/viewer` (Project Viewer):** Initially assigned to User 2. This primitive role granted read-only access to inspect the metadata state of all services (e.g., listing storage buckets) across the global project hierarchy without modification rights.
* **`roles/storage.objectViewer` (Storage Object Viewer):** Later assigned as a granular, resource-specific role to User 2. This granted precise access to list and download object blobs within Cloud Storage, without exposing other project hierarchies.

### 2. Infrastructure Resources
* **Cloud Storage Bucket:** A globally unique flat namespace container created by User 1 to hold an object payload (`sample.txt`) for cross-credential permissions verification.

---

## Evidence

### Multi-User Policy Enforcement Logs & Responses
To validate how Cloud IAM restricts operations, the environment's state was tested across different permission shifts.

**Scenario A: Global Revocation (User 2 Stripped of Project Viewer Role)**
When User 2 attempted to view project resources after their primitive `roles/viewer` status was deleted by the owner, the Console API blocked the request:
```text
Console UI Alert Message:
⚠️ You do not have permission to view this resource. 
Required permission(s): storage.buckets.list
### Evidence Reflection
This test illustrated the practical mechanics of IAM propagation delays and role boundaries. Primitive project-level roles are inherently broad and present an unnecessary security risk for specialized workers. Transitioning User 2 from a global Project Viewer to a specific Storage Object Viewer proved how enterprise security teams can limit an identity's blast radius, ensuring that a compromised account only exposes targeted data pools rather than structural project metadata.
