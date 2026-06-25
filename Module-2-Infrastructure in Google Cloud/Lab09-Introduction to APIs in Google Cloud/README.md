## Lab 9: Introduction to APIs in Google Cloud

### Objective
The objective of this lab was to dissect the core architectural design and communication lifecycle of web-based APIs within Google Cloud. The lab analyzed the properties of RESTful systems, JSON serialization, and the distinct modalities of API authentication (API Keys, OAuth2, and Service Accounts). These conceptual frameworks were practically verified by executing low-level REST calls inside Google Cloud Shell using the native HTTP protocol (`curl`), programmatically provisioning a Cloud Storage bucket, and uploading media blobs via standard multi-part payload vectors.

### Tools & Services Used
* **API Management Core:** Google Cloud API Library & APIs Explorer Engine
* **Protocol Testing Tool:** Linux Client Command-Line `curl` Utility
* **Authentication Provider:** Google OAuth 2.0 Playground Engine
* **Target Storage Pool:** Google Cloud Storage (JSON/REST API v1 Enpoints)

---

### Key Architectural Concepts

#### 1. RESTful API Blueprint (Representational State Transfer)
* **Client-Server Architecture:** Web APIs function over a stateless layout where a client sends structured requests over standard URI pathways, and an isolated backend host processes and satisfies the request.
* **HTTP Verbs (Operations Matrix):**
  * `GET`: Fetches a target resource from the server host without modifying state.
  * `POST`: Ingests input parameters to spawn or append a completely new cloud resource.
  * `PUT`: Completely overrides an existing dataset or instantiates a new resource if the target locator does not exist.
  * `DELETE`: Targets and drops specific computational records or objects from the cloud cluster.

#### 2. Authentication Taxonomy
* **API Keys:** Lightweight, encrypted token strings that identify the specific *calling project* to track billing quotas and enforce system throttling. They do not hold unique identity parameters.
* **OAuth 2.0 Tokens:** High-security tokens governed explicitly by a granular scope matrix. These tokens link directly to an individual *user account identity* to authorize or limit raw human-data manipulation.
* **Service Accounts:** Specialized architectural identities assigned natively to *applications or machines* (e.g., standard virtual machines or serverless triggers) rather than human operators, utilizing cryptographic private keys for verification.

---

### Step-by-Step Practical Implementation

#### 1. Ingestion Payload Mapping (`values.json`)
Before issuing a creation call, a standard metadata file specifying target resource characteristics was written to the local disk workspace:
```json
{
  "name": "qwiklabs-gcp-02-5d551758b5a7-bucket",
  "location": "us",
  "storageClass": "multi_regional"
}

#### 2. RESTful Resource Provisioning (Bucket Lifecycle Integration)
Using an authenticated short-lived OAuth2 bearer token, a raw HTTP POST request was dispatched to the standard Cloud Storage API endpoint wrapper:
curl -X POST --data-binary @values.json \
    -H "Authorization: Bearer $OAUTH2_TOKEN" \
    -H "Content-Type: application/json" \
    "[https://www.googleapis.com/storage/v1/b?project=$PROJECT_ID](https://www.googleapis.com/storage/v1/b?project=$PROJECT_ID)"
#### 3. Binary Media Stream Upload Execution (Object Lifecycle Integration)
To transfer an asset file (demo-image.png) into the object storage repository, a media upload URI modifier path query (uploadType=media) was called:
curl -X POST --data-binary @$OBJECT \
    -H "Authorization: Bearer $OAUTH2_TOKEN" \
    -H "Content-Type: image/png" \
    "[https://www.googleapis.com/upload/storage/v1/b/$BUCKET_NAME/o?uploadType=media&name=demo-image](https://www.googleapis.com/upload/storage/v1/b/$BUCKET_NAME/o?uploadType=media&name=demo-image)"
