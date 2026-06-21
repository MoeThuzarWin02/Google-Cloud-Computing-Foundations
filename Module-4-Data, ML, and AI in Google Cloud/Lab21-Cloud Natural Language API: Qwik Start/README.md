## Lab 21: Cloud Natural Language API: Qwik Start

### 📌 Objective
The objective of this lab was to explore the foundational capabilities of Natural Language Processing (NLP) within Google Cloud using the Cloud Natural Language API. Specifically, the lab demonstrated how to programmatically analyze unstructured text to perform **Entity Analysis**, allowing an application to automatically identify and categorize real-world subjects (such as people, locations, and events) and evaluate their relevance (salience) within the text.

### 🛠️ Tools & Services Used
* **AI & Machine Learning:** Cloud Natural Language API
* **Compute:** Compute Engine (Linux VM Instance used as the development environment), Cloud Shell
* **Identity & Security:** IAM (Identity and Access Management) Service Accounts, API Credentials

### ⚙️ Key Configurations
* **Service Account:** Created a dedicated service account (`my-natlang-sa`) to handle secure authentication requests to the ML API.
* **Environment Variable:** Configured `GOOGLE_APPLICATION_CREDENTIALS` pointing to a local `~/key.json` security key file to authenticate gcloud CLI requests from within the VM.
* **API Call Parameters:** Executed an entity analysis via `gcloud ml language analyze-entities` against the target string: *"Michelangelo Caravaggio, Italian painter, is known for 'The Calling of Saint Matthew'."*

### 💡 Reflection & Challenges
* **What I learned:** I learned how Google Cloud uses pre-trained machine learning models to extract metadata without needing to build or train a custom NLP model. I also came to understand the concept of **salience**, which measures how central a specific entity is to the context of the overall sentence.
* **Issues faced & Resolution:** Initially, I ran the service account setup in Cloud Shell instead of the SSH session of the provisioned Linux instance. I resolved this by ensuring I logged directly into the Compute Engine VM via SSH as instructed in Task 2 before triggering the `gcloud ml language` extraction commands.
