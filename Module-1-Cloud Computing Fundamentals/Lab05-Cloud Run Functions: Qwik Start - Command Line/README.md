# Lab 5: Cloud Run Functions: Qwik Start - Command Line

## Summary of the Lab
This lab introduced the fundamentals of event-driven architecture using **Cloud Run Functions (Generation 2)**, Google Cloud's premier serverless Function-as-a-Service (FaaS) platform. The lab walked through creating a stateless JavaScript function using the Node.js runtime, defining asynchronous message triggers, and deploying it programmatically using the `gcloud` command-line tool within Cloud Shell.

## Lab Objective
The core objective was to build and launch an event-driven system where code executes entirely in response to infrastructure mutations—specifically, messages arriving on a asynchronous messaging pipeline. This involved installing the Google Functions Framework dependency locally, packaging the app code, mapping it to a custom **Pub/Sub** messaging channel, and reviewing backend system logs to confirm execution.

---

## Key Configurations

### 1. Function Code Artifact Structure
* **`index.js` Configuration:** Implemented the `@google-cloud/functions-framework` library using a `cloudEvent` callback named `helloPubSub`. The code reads inbound Pub/Sub message bodies, unpacks the base64-encoded binary data payload, and logs the resulting cleartext message string.
* **`package.json` Configuration:** Set up the Node.js environment manifest, locking the Google Functions Framework dependency framework to target version version `^3.0.0`.

### 2. Deployment Architecture (Gen 2)
* **Execution Parameters:** Pushed via `gcloud functions deploy` leveraging the `--gen2` flag, which automatically configures the source code to package into an OCI container image using Cloud Build, stores it inside Artifact Registry, and runs it on top of Google's managed Knative engine (Cloud Run).
* **Trigger Mechanics:** Attached the `--trigger-topic cf-demo` flag, forcing an event binding between the function routing engine and a designated Pub/Sub messaging highway.

---

## Evidence

### Event-Driven Execution and Telemetry Logging
To ensure the event handlers were operating correctly, an asynchronous message payload was published to the Pub/Sub topic to verify backend message routing.



**Scenario A: Function Deployment Verification**
The programmatic deployment routine compiled the application manifest successfully, placing the resource pool into an un-throttled, active status state:
```bash
$ gcloud functions describe nodejs-pubsub-function --region=REGION

Parsed Platform Metadata:
BuildConfig:
  entryPoint: helloPubSub
  dockerRegistry: ARTIFACT_REGISTRY
  dockerRepository: gcf-artifacts
ServiceConfig:
  availableMemory: 256M
  service: projects/qwiklabs-gcp-gsp080/locations/REGION/services/nodejs-pubsub-function
State: ACTIVE
