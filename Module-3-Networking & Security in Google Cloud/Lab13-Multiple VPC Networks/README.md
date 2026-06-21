
## Lab 13: Multiple VPC Networks

### Objective
The objective of this lab was to design, implement, and analyze complex multi-VPC networking architectures within Google Cloud. The lab focused on the construction of custom mode VPC networks, the application of targeted ingress firewall rule sets, and the mechanics of internal network isolation versus external public internet routing. Additionally, it explored advanced infrastructure configurations by building a virtual appliance instance equipped with multiple network interface controllers (Multi-NIC) to bridge distinct, non-overlapping routing domains.

### Tools & Services Used
* **Core Networking:** Virtual Private Cloud (VPC Custom Networks, Custom Subnets, and Internal DNS)
* **Network Security:** VPC Ingress Firewall Rules (ICMP, SSH, RDP)
* **Compute Infrastructure:** Compute Engine (Standard and Multi-NIC Virtual Machine Instances)
* **Management Utilities:** Cloud Shell (`gcloud` CLI platform tools, `ifconfig`, and `ip route` diagnostics)

### Key Configurations
The infrastructure deployment spanned three distinct VPC topologies across multiple subnets and firewall policies:

#### 1. Network Topology & Subnet Infrastructure
| Network Name | Mode | Subnet Name | Configured Region | Managed IP Address CIDR Block |
| :--- | :--- | :--- | :--- | :--- |
| `mynetwork` | Auto | *Pre-provisioned* | `Region_1` | Automatically Assigned |
| **`managementnet`** | Custom | `managementsubnet-1` | `Region_1` | `10.130.0.0/20` |
| **`privatenet`** | Custom | `privatesubnet-1` | `Region_1` | `172.16.0.0/24` |
| **`privatenet`** | Custom | `privatesubnet-2` | `Region_2` | `172.20.0.0/20` |

#### 2. Security & Access Firewall Policy Configurations
* **`managementnet-allow-icmp-ssh-rdp`**: Applied to `managementnet` allowing ingress `tcp:22`, `tcp:3389`, and `icmp` from all sources (`0.0.0.0/0`).
* **`privatenet-allow-icmp-ssh-rdp`**: Applied to `privatenet` allowing ingress `tcp:22`, `tcp:3389`, and `icmp` from all sources (`0.0.0.0/0`).

#### 3. Compute Instance Provisioning Matrix
* **`managementnet-vm-1`**: Machine Type `e2-micro`, attached to `managementnet` / `managementsubnet-1`.
* **`privatenet-vm-1`**: Machine Type `e2-micro`, attached to `privatenet` / `privatesubnet-1`.
* **`vm-appliance` (Multi-NIC Router)**: Machine Type `e2-standard-4` *(Required to support $>2$ interfaces)*.
  * `nic0`: Bound to `privatenet` / `privatesubnet-1`
  * `nic1`: Bound to `managementnet` / `managementsubnet-1`
  * `nic2`: Bound to `mynetwork` / `mynetwork`

---

### Evidence & Results

#### Connectivity Diagnostic Matrix (Ping Evaluations)
* **External IP Mapping:** All instances successfully responded to external public IP pings. This verified that public internet routing is uninhibited by isolated internal topologies, controlled solely by the configured ingress firewall filters.
* **Internal Private Networking Domains:** Pinging via internal IP addresses yielded the following networking behaviors:
  * `mynet-vm-1` to `mynet-vm-2` ➡️ **SUCCESSFUL** (Shared common `mynetwork` fabric).
  * `mynet-vm-1` to `managementnet-vm-1` ➡️ **FAILED (100% Packet Loss)** (Complete cryptographic and logical isolation across distinct custom VPCs).

#### Multi-NIC Appliance Local Route Validation
Inspecting the kernel routing tables directly within the `vm-appliance` configuration via `ip route` exposed the underlying layout of multi-homed attachments:
```text
default via 172.16.0.1 dev eth0
10.128.0.0/20 via 10.128.0.1 dev eth2
10.130.0.0/20 via 10.130.0.1 dev eth1
172.16.0.0/24 via 172.16.0.1 dev eth0
