# Snowflake-DBT-Airflow-ETL-pipeline

## Overview
Designed and implemented an end-to-end ETL pipeline that stages and transforms data using dbt, and structures it into a star schema on Snowflake with well-defined fact and dimension tables. Automated the entire workflow using Apache Airflow DAGs, ensuring seamless scheduling, orchestration, and monitoring of data pipelines.

Tech Stack and process overview
- Snowflake environment setup: warehouse, database, roles, user, schemas.
- dbt core integration with Snowflake.
- Data modeling using dbt (staging and transformation).
- Star schema design with fact and dimension tables.
- dbt macros for business logic integration.
- Generic and singular tests in dbt for data validation.
- Workflow orchestration using Apache Airflow.

## Prerequisites
- Docker
- Astro CLI
- Python 3.x

## Setup Instructions

### 1. Install Dependencies
```bash
pip install astronomer-cosmos
pip install apache-airflow-providers-snowflake
pip install dbt-core
pip install dbt-snowflake
```

### 2. Setup Astro CLI and Initialize Project
```bash
astro dev init
```

### 3. Add dbt to PATH
- Add `dbt.exe` to your system's PATH to use with Airflow.

### 4. Initialize dbt Project
```bash
cd dags
dbt init dbt_project
```
- Enter Snowflake account info, warehouse name, database, role, schema.
- Set `threads` to 10.

### 5. Configure dbt Project
- Update `dbt_project.yml`.
- Add the latest version of `dbt_utils` to `packages.yml`.

### 6. Snowflake Setup

```sql
-- Use the account admin role
USE ROLE ACCOUNTADMIN;

-- Create warehouse, database, and role
CREATE WAREHOUSE IF NOT EXISTS dbt_wh WITH WAREHOUSE_SIZE = 'XSMALL';
CREATE DATABASE IF NOT EXISTS dbt_db;
CREATE ROLE IF NOT EXISTS dbt_role;

-- Show grants on the warehouse
SHOW GRANTS ON WAREHOUSE dbt_wh;

-- Grant usage and access
GRANT USAGE ON WAREHOUSE dbt_wh TO ROLE dbt_role;
GRANT ROLE dbt_role TO USER sbishnoi26;
GRANT ALL ON DATABASE dbt_db TO ROLE dbt_role;

-- Switch to the new role
USE ROLE dbt_role;

-- Create schema in the new database
CREATE SCHEMA IF NOT EXISTS dbt_db.dbt_schema;
```

![alt text](https://github.com/sahilbishnoi26/Snowflake-DBT-Airflow-ETL-pipeline/blob/main/images/img1.png)

### 7. Build Data Models
- Extract data from source and mirror into staging tables.
- Transform models into fact and dimension tables using star schema.
- Create macros in the `macros` folder for fact table generation.
- Add generic and singular tests to validate data.

### 8. Run Docker and Start Airflow
```bash
# Ensure Docker is running
astro dev start
```

### 9. Configure Airflow
- Add Snowflake connection in Airflow with your account and warehouse info.

## Usage
- Use Airflow DAGs to run and monitor the ETL pipeline.
- dbt handles data staging, transformation, and testing.
- Snowflake stores the final star schema structure.

![alt text](https://github.com/sahilbishnoi26/Snowflake-DBT-Airflow-ETL-pipeline/blob/main/images/img2.png)


## Project Structure
```
├── dags/
│   ├── dbt/
│   │   ├── models/
│   │   │   ├── staging/
│   │   │   ├── marts/
│   │   │       ├── fact/
│   │   │       └── dimension/
│   │   ├── macros/
│   │   ├── tests/
│   │   └── dbt_project.yml
│   └── airflow_dag.py
├── Dockerfile
├── requirements.txt
└── README.md
```

## Notes
- Ensure Docker is running before starting Airflow.
- Validate dbt models using built-in tests.
- Monitor DAGs and task runs through Airflow UI.
