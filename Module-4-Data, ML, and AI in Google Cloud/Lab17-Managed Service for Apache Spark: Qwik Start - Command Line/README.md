## Lab 22: Managed Service for Apache Spark: Qwik Start - Command Line

### Objective
The objective of this lab was to provision, manage, and scale a fully managed Apache Spark and Hadoop cluster using **Cloud Dataproc** via the `gcloud` CLI. The lab demonstrated how to handle necessary IAM permissions for service accounts, enable Private Google Access on a VPC subnet, submit a computationally heavy Spark job, and dynamically scale cluster worker nodes up and down without causing operational downtime.

### Tools & Services Used
* **Big Data Processing:** Cloud Dataproc (Managed Apache Spark & Hadoop Engine)
* **Identity & Security:** IAM (Identity and Access Management Policy Bindings)
* **Networking:** Virtual Private Cloud (VPC Subnet configuration)
* **Management Tool:** Cloud Shell (gcloud CLI environment)

### Key Configurations
The cluster deployment required setting underlying storage roles and network permissions prior to provisioning infrastructure nodes:

| Stage | Action / Component | Value / Configuration Rule |
| :--- | :--- | :--- |
| **1** | IAM Bindings | Granted `roles/storage.admin` and `roles/dataproc.worker` to the Compute Engine default service account. |
| **2** | Subnet Update | Configured the default network with `--enable-private-ip-google-access` to allow nodes secure access to GCP APIs. |
| **3** | Cluster Name | `example-cluster` |
| **4** | Control Node Type | Master Machine: `e2-standard-4` |
| **5** | Processing Nodes | Primary Worker Machine: `e2-standard-4` (Disk Size: `500GB`) |

