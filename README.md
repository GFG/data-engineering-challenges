# Data Engineering Challenges

---

## Table of Contents

- [Assignment Overview](#assignment-overview)
  - [Scenario A: Input from Kafka, Output to Amazon S3](#scenario-a-input-from-kafka-output-to-amazon-s3)
  - [Scenario B: Input from BigQuery (BQ), Output to Amazon S3](#scenario-b-input-from-bigquery-bq-output-to-amazon-s3)
- [Part 1: Data Pipeline Implementation](#part-1-data-pipeline-implementation)
  - [Data Description](#data-description)
    - [Data Fields](#data-fields)
    - [Sample Data](#sample-data)
    - [Data Processing](#data-processing)
      - [Example of Transformed Data](#example-of-transformed-data)
  - [Optional Step: Monitoring and Alerting](#optional-step-monitoring-and-alerting)
- [Part 2: Infrastructure Deployment](#part-2-infrastructure-deployment)
- [Deliverables](#deliverables)
- [How to Submit the Assignment](#how-to-submit-the-assignment)
- [Additional Guidelines](#additional-guidelines)
- [Evaluation Criteria](#evaluation-criteria)

---

# Assignment Overview

Develop a data pipeline by choosing one of the following scenarios:

## Scenario A: Input from Kafka, Output to Amazon S3

- **Input Source**: Apache Kafka
- **Output Destination**: Amazon S3
- **Daily Data Volume**: Approximately 400-500 GiB
- **Task**: Ingest low-latency streaming data from Kafka, process it, and store the transformed data in Amazon S3.

## Scenario B: Input from BigQuery (BQ), Output to Amazon S3

- **Input Source**: Google BigQuery
- **Output Destination**: Amazon S3
- **Daily Data Volume**: Approximately 400-500 GiB
- **Task**: Extract data from BigQuery, process it, and load it into Amazon S3.

---

# Part 1: Data Pipeline Implementation

- **Select a Scenario**: Choose one scenario (A or B) to implement.

## Data Description

- **Data Type**: User action events.

### Data Fields

- **timestamp**: The time at which the event occurred.
- **event_name**: The type of the event (e.g., click, view, purchase).
- **user_identifiers**:
  - **platform**: The platform used by the user (e.g., web, mobile).
  - **id**: Unique identifier for the user.
- **product_information**:
  - **name**: Name of the product involved in the event.
  - **SKU**: Stock Keeping Unit identifier of the product.

### Sample Data

```json
{
  "timestamp": "2023-10-23T12:34:56Z",
  "event_name": "view",
  "user_identifiers": {
    "platform": "web",
    "id": "user_12345"
  },
  "product_information": {
    "name": "Wireless Mouse",
    "SKU": "SKU-1001"
  }
}
```

```json
{
  "timestamp": "2023-10-23T12:35:10Z",
  "event_name": "click",
  "user_identifiers": {
    "platform": "mobile",
    "id": "user_67890"
  },
  "product_information": {
    "name": "Bluetooth Headphones",
    "SKU": "SKU-2002"
  }
}
```

```json
{
  "timestamp": "2023-10-23T12:36:05Z",
  "event_name": "purchase",
  "user_identifiers": {
    "platform": "web",
    "id": "user_12345"
  },
  "product_information": {
    "name": "Mechanical Keyboard",
    "SKU": "SKU-3003"
  }
}
```

### Data Processing

- **Transformation Requirements**:
  - **Parsing and Validation**: Ensure that each record has all the required fields and that the data types are correct.
  - **Data Enrichment** (Optional): Enrich the data by adding additional fields, such as:
    - **event_date**: Extract the date from the timestamp.
  - **Handling Missing or Null Values**: Implement logic to handle records with missing or null values appropriately.
- **Output Format**:
  - Store the processed data in Amazon S3 in a partitioned format (e.g., partitioned by date or event type).
  - Use a columnar storage format like Parquet or ORC for efficient storage and query performance.

#### Example of Transformed Data

```json
{
  "event_datetime": "2023-10-23T12:34:56Z",
  "event_date": "2023-10-23",
  "event_name": "view",
  "user_id": "user_12345",
  "platform": "web",
  "product_name": "Wireless Mouse",
  "SKU": "SKU-1001"
}
```

## Optional Step: Monitoring and Alerting

- **Monitoring**: Implement monitoring for the data pipeline to track performance, data throughput, and any errors.
- **Alerting**: Set up alerts to notify of failures or performance issues.
- **Tools and Mechanisms**: Feel free to choose any tools or mechanisms, such as:
  - **Monitoring**:
    - Prometheus and Grafana
    - AWS CloudWatch
    - Elasticsearch and Kibana
  - **Alerting**:
    - Email notifications
    - Slack alerts
    - PagerDuty
- **Implementation**:
  - Integrate monitoring into your pipeline to collect metrics.
  - Configure alerting rules based on critical thresholds or error conditions.

---

# Part 2: Infrastructure Deployment

**Requirement**: Deploy your data pipeline using Apache Airflow on Kubernetes.

- **Tasks**:
  - **Airflow Setup**:
    - Prepare Kubernetes YAML files to set up Apache Airflow on the Kubernetes cluster.
    - Configure Airflow components including the Webserver, Scheduler, and Workers (if using CeleryExecutor or KubernetesExecutor).
    - Use Kubernetes resources such as Deployments, Services, ConfigMaps, and Secrets as needed.
  - **Pipeline Deployment**:
    - Create an Airflow DAG (Directed Acyclic Graph) that defines the data pipeline workflow.
    - Ensure the DAG includes tasks for data extraction, processing, and loading to S3.
  - **Pod Templates**:
    - Prepare pod templates or configurations for the tasks in your Airflow DAG if using the `KubernetesPodOperator` or similar.
    - Configure resource requests and limits for your pods.
    - Use necessary volume mounts, environment variables, and other Kubernetes features.
  - **CI/CD Integration** (Optional):
    - Include steps to deploy your Airflow setup and DAGs using a CI/CD pipeline if applicable.

---

# Deliverables

1. **Code Repository**:
   - All source code for the data pipeline, including Airflow DAGs.
   - Include a `README.md` with instructions on how to set up and run the pipeline.

2. **Infrastructure as Code**:
   - Kubernetes YAML files (`.yaml`) for setting up Apache Airflow on Kubernetes.
   - Pod templates or Kubernetes configurations for running pipeline tasks.
   - Ensure all configurations are well-documented.

3. **Repository Structure:**
  - Organize your repository using the following structure:
    ```
    - src/                    # Contains the pipeline code base
      - deploy/
          - terraform/          # Contains Terraform code if needed
          - kubernetes/         # Contains Kubernetes YAML files
      - data/                   # (Optional) Sample data files
    - README.md               # Instructions on how to set up and run the pipeline
    ```
4. **Sample Data**:
   - Include a set of sample data files (e.g., in a `data/` directory) that can be used to test the pipeline.
   - Provide instructions on how to use the sample data.

5. **Documentation**:
   - Explanation of design choices and architecture.
   - Any assumptions made during implementation.
   - Instructions for deploying Airflow on Kubernetes and running the pipeline.
   - Details on how to monitor and alert on the pipeline's performance and health.

---

# How to Submit the Assignment
Please follow the steps below to submit your assignment:

1. **Create a Public GitHub Repository:**
  - Host your code and all related files in a new public repository on GitHub.
  - Ensure the repository includes all the deliverables specified in the Deliverables section.

2. **Add Our Team as Collaborators:**
  - Add the following GitHub usernames as collaborators to your repository so we can review your work:
    - @trinhle-gfg
    - @tri-tran-gfg
    - @ngwanzhen
  - To add collaborators:
    - Go to your repository's Settings.
    - Navigate to Manage Access (or Collaborators).
    - Click on Invite a collaborator and enter the usernames above.

3. **Provide Access Instructions (if necessary):**
  - If any parts of your project require special instructions to access or run (e.g., API keys, credentials), include mock values and explain how to replace them in your README.md.
  - Do not include any sensitive information in your repository.

4. **Notify Us:**
  - Once your repository is ready, please notify us by sending an email or message with the link to your GitHub repository.
  - Include any additional notes or instructions you think are necessary.
---

# Additional Guidelines

- **Data Handling**:
  - Ensure idempotency and fault tolerance in data processing.
  - Implement retry mechanisms for transient failures.
  - Handle exceptions and edge cases gracefully.

- **Performance Optimization**:
  - **Data Partitioning**:
    - Partition data by date, event type, or other relevant fields to improve query performance.
  - **Efficient Storage**:
    - Use columnar data formats and compression to reduce storage size and I/O operations.
  - **Concurrency**:
    - Leverage parallel processing capabilities of the chosen framework.

- **Monitoring and Alerting**:
  - Implement monitoring to track the health and performance of the data pipeline.
  - Set up alerting mechanisms to notify responsible parties of failures or degraded performance.
  - Document the monitoring and alerting setup in your documentation.

- **Security and Compliance**:
  - Manage sensitive information using Kubernetes Secrets or environment variables.
  - Secure access to data sources and destinations with proper authentication and authorization.
  - Ensure compliance with data protection regulations.

- **Testing**:
  - Include unit tests and integration tests for your data processing code.
  - Provide end-to-end testing instructions.
  - Use the provided sample data to simulate the ingestion and processing of user action events.

---

# Evaluation Criteria

- **Functionality**: Correct implementation of the chosen scenario.
- **Data Handling**: Accurate processing of user action events with all specified fields.
- **Scalability**: Ability to handle large data volumes efficiently.
- **Airflow Deployment**: Effective deployment and configuration of Airflow on Kubernetes.
- **Monitoring and Alerting**: Implementation of monitoring and alerting mechanisms.
- **Code Quality**: Clean, readable, and maintainable code with proper documentation.
- **Infrastructure**: Accurate and efficient deployment configurations using Kubernetes.
- **Documentation**: Clarity and completeness of the provided documentation.

---
