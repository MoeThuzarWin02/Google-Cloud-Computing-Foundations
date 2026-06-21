# Lab 11: User Authentication: Identity-Aware Proxy

## Summary of the Lab
This lab focused on moving away from traditional perimeter network security and implementing a modern, zero-trust user authentication framework on Google Cloud Platform. By routing traffic through a managed proxy layer, a serverless application can safely determine exactly who a user is and verify that identity cryptographically before granting access to sensitive backends.

## Lab Objective
The objective was to deploy a Python Flask application on **Cloud Run** and secure it using **Identity-Aware Proxy (IAP)**. The core engineering goal was to extract user identities from proxy-injected HTTP headers and protect the application from security risks—like proxy bypass or header spoofing—by implementing cryptographically signed **JSON Web Token (JWT)** verification within the application code.

---

## Key Configurations

### 1. Identity & Access Management (IAM)
* **`IAP-Secured Web App User`:** Explicitly granted to the authorized user principal account to pass edge proxy validation.
* **`Cloud Run Invoker`:** Assigned to the internal IAP service agent (`service-PROJECT_NUMBER@gcp-sa-iap.iam.gserviceaccount.com`). This configuration allows the proxy gateway to securely pass traffic down to the underlying Cloud Run service instance after validating the user.

### 2. Runtime Environment Variables
* **`IAP_AUDIENCE`:** Set as a localized container environment variable matching the unique **Signed Header JWT Audience Client ID**. This value is used by the Python runtime environment to confirm that incoming JWT assertions were specifically intended for this exact application deployment.

---

## Evidence

### Verified vs. Spoofed Header Simulation Logs
To validate the security framework, an exfiltration test was simulated by turning off IAP and sending a rogue client request containing a malicious header injection via `curl`. 

**Scenario A: Vulnerable Configuration (Proxy Bypassed / Raw Headers Trusted)**
When evaluating unverified headers, the application blindly ingested the client-supplied parameters:
```text
$ curl -X GET $CLOUD_RUN_URL -H "X-Goog-Authenticated-User-Email: totally fake email"

Parsed Application Log:
-> Extracting Unverified Email Header: "totally fake email"
-> Extracting Cryptographic User ID: None (JWT Assertion missing)
❌ SECURITY VULNERABILITY: Application accepted the fake identity context.
### Evidence Reflection
This test highlighted the risk of trusting structural data headers without signature validation. In a zero-trust model, the backend cannot assume its boundary firewall is infallible; it must dynamically enforce cryptographic checks. Moving authentication to the cloud edge via IAP simplifies local codebases, while verifying the asymmetric `ES256` signature ensures that identity injection attacks are fundamentally neutralized even if perimeter security configurations are altered or bypassed.
