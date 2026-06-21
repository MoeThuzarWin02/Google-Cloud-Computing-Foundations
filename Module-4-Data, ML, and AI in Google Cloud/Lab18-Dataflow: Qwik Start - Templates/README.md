## Lab 18: Dataflow: A Streaming Pipeline - Pub/Sub to BigQuery

### Objective
The objective of this lab was to deploy a fully managed, real-time streaming ETL (Extract, Transform, Load) data pipeline using a Google-provided Dataflow template. The pipeline ingests live JSON-formatted streaming data from a public **Cloud Pub/Sub** messaging topic (`taxirides-realtime`), processes it mid-flight via **Cloud Dataflow**, and dynamically streams the structured output directly into a partitioned **BigQuery** data warehouse table for instant analytical querying.

### Tools & Services Used
* **Data Ingestion:** Cloud Pub/Sub (Public stream source)
* **Stream Processing:** Cloud Dataflow (Pub/Sub to BigQuery Streaming Template)
* **Data Warehousing:** BigQuery (Dataset creation, table structuring, and SQL Engine)
* **Storage & Management:** Cloud Storage (GCS Bucket used as pipeline scratch/staging space), Cloud Shell

### Key Configurations
* **Data Lifecycle Assets:** Created a BigQuery dataset named `taxirides` containing a streaming target table called `realtime`.
* **Table Schema & Strategy:** Modeled the streaming schema using a comma-separated column definitions script, explicitly utilizing time-based partitioning on the `timestamp` field to optimize query execution and storage costs:
  ```text
  ride_id:string,point_idx:integer,latitude:float,longitude:float,timestamp:timestamp,meter_reading:float,meter_increment:float,ride_status:string,passenger_count:integer
