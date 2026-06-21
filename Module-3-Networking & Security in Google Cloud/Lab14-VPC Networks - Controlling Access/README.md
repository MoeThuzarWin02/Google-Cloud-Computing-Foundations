## Lab 14: VPC Networks - Controlling Access

### Objective
The objective of this lab was to implement granular network access controls and apply identity security mechanisms within a Virtual Private Cloud (VPC) network. The lab demonstrated how to isolate public HTTP ingress traffic to specific workload layers using **Network Tags**, configure redundant web server instances running Nginx (`blue` and `green`), and analyze the security principle of least privilege by evaluating the practical permission boundaries of the **Compute Network Admin** and **Compute Security Admin** IAM roles via authenticated service account keys.

### Tools & Services Used
* **Compute & Workloads:** Compute Engine (Nginx-Light Web Server Nodes: `blue` and `green`)
* **Network Security:** VPC Target-Tagged Firewall Rules & Default Network Fabric
* **Identity & Access Management:** IAM Service Accounts, JSON Key Generation, and Custom Policy Evaluation
* **Diagnostic Tools:** Cloud Shell, `curl` HTTP connection testers, and `gcloud auth` credential activators

### Key Configurations
The architectural implementation relied on tagging networks and binding service accounts to precise roles:

#### 1. Compute Instance & Network Tag Matrix
| Instance Name | Machine Type | Subnet | Applied Network Tags | Initialized Software |
| :--- | :--- | :--- | :--- | :--- |
| `blue` | `e2-micro` | `default` | **`web-server`** | `nginx-light` (Customized Page) |
| `green` | `e2-micro` | `default` | *None (Blank)* | `nginx-light` (Customized Page) |
| `test-vm` | `e2-micro` | `default` | *None (Blank)* | Cloud SDK Diagnostic Client |

#### 2. Firewall Access Rules
* **`allow-http-web-server`**: Ingress directive allowing `tcp:80` and `icmp` from any source (`0.0.0.0/0`). Critically, the **Target Specificity** was restricted exclusively to instances carrying the **`web-server`** network tag.

#### 3. IAM Service Account Progression
* **Service Account ID:** `Network-admin`
* **Role Transition State:** Promoted from `roles/compute.networkAdmin` (Compute Network Admin) up to `roles/compute.securityAdmin` (Compute Security Admin) to acquire structural modification access privileges.

---

### Evidence & Results

#### Connectivity Matrix (Internal vs. External Access Validation)
* **Internal Routing Layer:** From the isolated `test-vm`, executing a `curl` against both the internal IP of `blue` and `green` succeeded. This proved that the pre-existing `default-allow-internal` firewall rule allows unimpeded cross-instance communication inside the same VPC network.
* **External Routing Layer:** Requesting the public external IP of the instances yielded an asymmetric result:
  * `curl <blue-external-ip>` ➡️ **SUCCESSFUL** (Returned `"Welcome to the blue server!"`)
  * `curl <green-external-ip>` ➡️ **FAILED (Hangs / Request Timeout)** (Blocked due to missing network tag).

#### IAM Permission Evaluation Signatures
* **Default State:** Running `gcloud compute firewall-rules list` inside `test-vm` threw an `Insufficient Permission` block error.
* **Network Admin Authorized State:** Successfully listed the firewall topology but failed with a severe error when executing a removal request:
  ```text
  ERROR: (gcloud.compute.firewall-rules.delete) Could not fetch resource:
   - Required 'compute.firewall.delete' permission...
