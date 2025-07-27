# ETL Toll Data Pipeline Project – IBM Data Engineering Professional Certificate

## Overview

This project demonstrates the development of an ETL (Extract, Transform, Load) pipeline using **Apache Airflow** and **BashOperator** to automate toll data processing across diverse file formats. It was completed as the final project for **Course 8 – ETL and Data Pipelines with Shell, Airflow, and Kafka** in the [IBM Data Engineering Professional Certificate](https://www.coursera.org/professional-certificates/ibm-data-engineer).

---

## Objectives

- Extract data from CSV, TSV, and fixed-width file formats
- Transform and consolidate traffic toll data
- Automate the entire ETL process using Apache Airflow DAGs and Bash
- Output a single cleaned and transformed dataset for staging and further analysis

---

## Tools & Technologies

| Category                   | Tools/Technologies                         |
| -------------------------- | ------------------------------------------ |
| **Workflow Orchestration** | Apache Airflow, BashOperator               |
| **Scripting & Utilities**  | Bash (`cut`, `paste`, `tr`, `tar`, `curl`) |
| **File Formats**           | CSV, TSV, Fixed-width text                 |

---

## ETL Pipeline Components

### 1. Unzip Data

- Unpacks `tolldata.tgz` into a working directory using `tar`.
- This is the first step to make the raw data files accessible to subsequent tasks.

### 2. Extract Data from CSV

- Extracts `Rowid`, `Timestamp`, `Anonymized Vehicle number`, and `Vehicle type` from `vehicle-data.csv`.
- Uses the `cut` command to select only the first four columns of the CSV.

### 3. Extract Data from TSV

- Extracts `Number of axles`, `Tollplaza id`, and `Tollplaza code` from `tollplaza-data.tsv`.
- Fields are tab-separated; `cut -f5-7` selects the required columns.

### 4. Extract Data from Fixed-Width File

- Extracts `Type of Payment code` and `Vehicle Code` from specific character ranges in `payment-data.txt`.
- Uses `cut -c59-67` to grab the character positions and `tr` to clean spacing.

### 5. Consolidate Extracted Data

- Merges all extracted files into `extracted_data.csv` using the `paste` command.
- This horizontally combines the extracted columns from all sources into one file.

### 6. Transform Data

- Capitalizes all values in the `vehicle_type` column using `tr`.
- The result is saved as `transformed_data.csv` in the staging area.

---

## Final Output

The pipeline outputs a cleaned, consolidated file:

```
data/staging/transformed_data.csv
```

This file contains the following fields:

- **Rowid**
- **Timestamp**
- **Anonymized Vehicle number**
- **Vehicle type** (converted to uppercase)
- **Number of axles**
- **Tollplaza id**
- **Tollplaza code**
- **Type of Payment code**
- **Vehicle Code**

---

## DAG Configuration

- **DAG ID**: `ETL_toll_data`
- **Schedule**: Daily
- **Owner**: Roy
- **Retries**: 1 (with 5-minute delay)
- **Email alerts**: Enabled for failure and retry

---

## Repository Structure

```plaintext
ETL_Toll_Data_Pipeline_Project/
├── README.md                         # Project overview, objectives, tools, and instructions
├── airflow/
│   └── ETL_toll_data.py              # Final Airflow DAG script automating the ETL pipeline
├── data/
│   ├── raw/                          # Original input files extracted from tolldata.tgz
│   │   ├── vehicle-data.csv          # Vehicle metadata (CSV format)
│   │   ├── tollplaza-data.tsv        # Toll plaza details (TSV format)
│   │   ├── payment-data.txt          # Payment info in fixed-width text format
│   ├── staging/                      # Intermediate and final output from ETL pipeline
│   │   ├── csv_data.csv              # Extracted columns from vehicle-data.csv
│   │   ├── tsv_data.csv              # Extracted columns from tollplaza-data.tsv
│   │   ├── fixed_width_data.csv      # Extracted fields from payment-data.txt
│   │   ├── extracted_data.csv        # Consolidated output of all extracted sources
│   │   └── transformed_data.csv      # Final cleaned and capitalized output file
├── docs/
│   └── fileformats.txt               # Provided documentation for file formats and field positions
├── images/                           # Screenshots used for verification and documentation of DAG setup, execution, and task success
│   ├── dag_paused.png                # DAG listed in Airflow but paused
│   ├── dag_active.png                # DAG unpaused and active
│   ├── dag_tasks_success.png         # Visual confirmation of successful task execution
│   ├── dag_run_triggered.png         # Screenshot of DAG run and timing info
```

---

## How to Run the DAG

1. Place the entire project folder inside your Airflow DAGs directory:

   ```
   ~/airflow/dags/ETL_Toll_Data_Project/
   ```

2. Launch the Airflow UI in your browser and ensure the DAG `ETL_toll_data` appears.

3. Unpause the DAG using the toggle switch.

4. Trigger the DAG manually from the Airflow UI or let it run according to its schedule.

5. After the DAG run completes, the output file will be located at:

   ```
   data/staging/transformed_data.csv
   ```

---

## License

This project was completed as part of the IBM Data Engineering Professional Certificate and is intended for educational use.

## Links

- Course Page - [ETL and Data Pipelines with Shell, Airflow, and Kafka](https://www.coursera.org/learn/etl-and-data-pipelines-shell-airflow-kafka)
- [GitHub Profile](https://github.com/royungar)
- [GitHub Repository](https://github.com/royungar/ETL_Toll_Data_Pipeline_Project)