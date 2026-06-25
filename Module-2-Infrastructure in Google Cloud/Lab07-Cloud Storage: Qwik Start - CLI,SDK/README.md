
## Lab 7: Cloud Storage: Qwik Start - CLI/SDK

### Objective
The objective of this lab was to manage unstructured data assets programmatically using the Google Cloud CLI (`gcloud storage`). The lab demonstrated how to provision a globally unique Cloud Storage bucket, execute object ingestion and extraction routines, implement simulated folder structures within a flat namespace, and adjust Access Control Lists (ACLs) to toggle public object exposure while adhering to the security principle of data lifecycle control.

### Tools & Services Used
* **Data Storage Fabric:** Google Cloud Storage (Object Storage Layer)
* **Command-Line Interface:** Cloud Shell & Google Cloud CLI (`gcloud storage` Component)
* **External Assets:** Wikimedia Public Repository (Inbound test image download payload)

---

### Key Configurations & CLI Operations

#### 1. Global Namespace Provisioning
* **Syntax Blueprint:** `gcloud storage buckets create gs://<YOUR-BUCKET-NAME>`
* **System Execution Matrix:** Buckets are created with a default Standard storage class and inherited IAM policy structures unless specific regional flags are added.

#### 2. Local-to-Remote Data Ingestion
* **Payload Fetch:** Downloaded a target image file (`ada.jpg`) to the temporary Cloud Shell local storage using `curl`.
* **Upload Pipeline Execution:**
  ```bash
  gcloud storage cp ada.jpg gs://YOUR-BUCKET-NAME
