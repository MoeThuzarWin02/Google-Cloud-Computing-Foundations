## Lab 19: Dataflow: Qwik Start - Python

### Objective
The objective of this lab was to configure a Python development environment to build and deploy serverless data processing pipelines. Using the open-source **Apache Beam SDK**, the lab demonstrated how to execute a data pipeline both locally using a `DirectRunner` and scale it out remotely on managed cloud infrastructure using **Google Cloud Dataflow** (`DataflowRunner`) backed by Cloud Storage.

### Tools & Services Used
* **Data Pipelines:** Google Cloud Dataflow, Apache Beam SDK (Python)
* **Storage:** Cloud Storage (GCS Buckets used for pipeline staging, temp space, and output destinations)
* **Containers & Shell:** Docker (Python 3.12 Isolation Image), Cloud Shell

### Key Configurations
* **API Dependencies:** Toggled the `dataflow.googleapis.com` service framework to clear and ensure full backend activation.
* **Storage Bucket:** Created a globally unique, multi-regional (`us`) GCS storage asset named `____-bucket`.
* **Runtime Isolation:** Initialized a local execution context using a standard interactive Python Docker environment:
  ```bash
  docker run -it -e DEVSHELL_PROJECT_ID=$DEVSHELL_PROJECT_ID python:3.12 /bin/bash
