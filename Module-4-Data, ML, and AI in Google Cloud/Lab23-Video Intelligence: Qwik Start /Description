# Lab 23 – Analyze Videos with the Video Intelligence API

## Objective

The objective of this lab was to learn how to use the Google Cloud Video Intelligence API to analyze video content automatically. The lab also demonstrated how to authenticate using a custom service account and send API requests through Cloud Shell.

---

## Summary

In this lab, I configured a service account for authentication and used the Cloud Video Intelligence API to analyze a video stored in Google Cloud Storage. The API processed the video and returned metadata such as detected labels and objects found throughout the video.

---

## Google Cloud Services Used

- Cloud Shell
- Identity and Access Management (IAM)
- Service Accounts
- Cloud Video Intelligence API
- Cloud Storage

---

## Key Configurations

### 1. Created a Service Account

Created a new service account named **quickstart** using the gcloud command.

### 2. Generated a Service Account Key

Created a JSON key file for authentication.

### 3. Authenticated the Service Account

Activated the service account using the generated key file.

### 4. Requested an Access Token

Generated an OAuth access token to authorize API requests.

### 5. Created a Video Annotation Request

Prepared a JSON request specifying:

- Input video stored in Cloud Storage
- LABEL_DETECTION feature

### 6. Sent the API Request

Used the curl command to send a request to the Video Intelligence API.

### 7. Retrieved the Results

Checked the operation status until the video analysis completed successfully.

---

## Key Commands

```bash
gcloud iam service-accounts create quickstart
gcloud iam service-accounts keys create key.json
gcloud auth activate-service-account --key-file key.json
gcloud auth print-access-token
curl https://videointelligence.googleapis.com/v1/videos:annotate
