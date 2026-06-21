## Lab 20: Dataprep by Alteryx Designer Cloud: Qwik Start

### Objective
The objective of this lab was to utilize **Dataprep by Alteryx Designer Cloud** to visually explore, clean, transform, and aggregate data without writing code. Using United States Federal Elections Commission (FEC) 2016 campaign data, the lab demonstrated how to import disparate raw data formats from Cloud Storage, rectify mismatched data typings, join tables based on inferred relational keys, and generate a metrics-driven summary table.

### Tools & Services Used
* **Analytics & Data Prep:** Dataprep by Alteryx Designer Cloud (Trifacta)
* **Storage:** Cloud Storage (GCS Bucket creation and source data ingestion)
* **Management & CLI:** Cloud Shell (Service Identity initialization)

### Key Configurations
* **Storage Asset:** Provisioned a unique, regional Cloud Storage bucket to act as the working file path repository.
* **Flow Name:** `FEC-2016` (Description: *United States Federal Elections Commission 2016*)
* **Ingested Datasets:** * `gs://spls/gsp105/us-fec/cn-2016.txt` (Renamed to *Candidate Master 2016*)
  * `gs://spls/gsp105/us-fec/itcont-2016-orig.txt` (Renamed to *Campaign Contributions 2016*)
* **Column Data Modification:** Changed the data type of `Column6` from `State` (geographic flag) to a standard `String` to eliminate mismatch conflicts caused by non-state entries like "US".
* **Join Mapping Key:** Performed an inner-style join between datasets mapping `column2` (Candidate Master) to `column11` (Campaign Contributions).

