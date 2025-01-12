# Data-pipelining-on-cloud

## Project Overview

This project demonstrates the implementation of a modern ELT (Extract, Load, Transform) pipeline leveraging cloud-based tools. The primary objective is to load raw data into Snowflake, transform it using DBT, and orchestrate the pipeline using Airflow. This repository provides an overview of the steps, components, and error-handling strategies utilized in the project.

## Conceptual Background

- **ETL** (Extract, Transform, Load): Historically optimized for high storage costs by transforming data before loading it into the data warehouse.
- **ELT** (Extract, Load, Transform): Modern approach enabled by cost-effective storage solutions like Snowflake, allowing raw data to be loaded first and transformed later.


## Tools Used

- **Snowflake**: Cloud-based data warehousing.
- **DBT Core**: For transforming and modeling data.
- **Airflow**: For orchestration and automation of data pipelines.
- **Astronomer Cosmos**: To run DBT projects within Airflow DAGs.

## Steps in the ELT Pipeline

1. Environment Setup
   - Snowflake Configuration:
     - Created a warehouse (DBT Warehouse), database (DBT DB), schema (DBT Schema), and role (DBT Role).
     - Assigned permissions for the role to access the warehouse and database.
     - Verified setup using Snowflake’s UI and SQL queries.
       
    - DBT Installation:
       - Installed DBT Core using pip install dbt-core.
       - Configured DBT profiles to connect with Snowflake, specifying roles, schemas, and warehouses.
         
   - Airflow Integration:
      - Initialized an Airflow project using Astronomer’s CLI.
      - Configured Docker and added dependencies for DBT and Snowflake.

<br/>

2. DBT Project Structure

├── dbt_project <br/>
│    &nbsp;  ├── models <br/>
│    &nbsp;  │   ├── staging <br/>
│    &nbsp;  │   ├── marts <br/>
│    &nbsp;  │   └── tests <br/>
│    &nbsp;  ├── macros <br/>
│    &nbsp;  ├── snapshots <br/>
│    &nbsp;  ├── seeds <br/>
│    &nbsp;  └── dbt_project.yml <br/>

- Staging Models: Contain one-to-one mappings to source tables, materialized as views.

- Mart Models: Aggregated and transformed data materialized as tables.

- Macros: Reusable functions for common business logic.

- Tests: Validated data integrity through generic and singular tests.

<br/>

3. Building and Running the Pipeline

   3.1 Source Tables:
   - Defined in Snowflake’s tpch dataset.
   - Validated primary and foreign keys through generic tests.

   3.2 Staging Models:
   - Extracted and cleaned raw data.
   - Standardized column names and generated surrogate keys.

   3.3 Mart Models:
   - Applied business logic for aggregation and transformation.
   - Built fact tables using dimensional modeling principles.

   3.4 Testing Framework
   - Generic Tests:
     - Validated uniqueness and null constraints for primary keys.
     - Checked acceptable values for specific columns.
   - Singular Tests:
     - Verified custom business rules, such as ensuring non-negative discounts and valid date ranges.

<br/>

## Orchestration

airflow_project/ <br/>
├── dags/<br/>
│  &nbsp;    └── dbt_dag.py <br/>
├── Dockerfile <br/>
└── requirements.txt <br/>

- Airflow DAG:

  - Integrated DBT models using Astronomer Cosmos.

  - Scheduled daily runs for pipeline tasks.

  - Configured Snowflake connections for the Airflow environment.

- Execution:

  - Visualized DAG dependencies and monitored execution through Airflow’s UI.

  - Verified successful runs by checking outputs in Snowflake.



## Error Handling Strategies

- Snowflake Configuration Errors

  - Issue: Permissions not granted to the role.

  - Resolution: Verified and granted necessary permissions for warehouses and databases.

- DBT Syntax Errors

  - Issue1: Incorrect YAML file formatting (e.g., missing indentations).

  - Resolution: Corrected formatting and validated YAML structure before rerunning.

  - Issue2: Deprecated DBT utility functions.

  - Resolution: Updated function names as per DBT documentation.

- Pipeline Execution Errors

  - Issue1: Incorrect Snowflake connection credentials in Airflow.

  - Resolution: Updated Airflow connection details for username, password, and account settings.

  - Issue2: Circular dependencies in DAG.

  - Resolution: Reviewed and reordered DBT models to ensure no interdependencies.

- Test Failures

  - Issue: Failing tests for invalid data values.

  - Resolution: Corrected data or adjusted transformation logic to meet business rules.

- Orchestration Issues

  - Issue: Airflow DAG not recognizing DBT project.

  - Resolution: Verified path configurations and reloaded the Airflow environment.



This document serves as a comprehensive reference for implementing scalable and robust ELT pipelines in a modern data engineering ecosystem.

