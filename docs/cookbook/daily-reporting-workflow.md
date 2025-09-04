# Cookbook: Automating a Daily Report

This recipe demonstrates a complete, real-world workflow: fetching data from a database, transforming it into a professional Excel report and an HTML email summary, and preparing it for delivery.

This showcases the synergy between nearly every major feature of the `cx` shell: connections, queries, variables, pipelines, and formatters.

**Goal:** Create an Excel report and an HTML email body from a SQL query.

---

### Prerequisites

1.  A saved `.sql` file in `~/.cx/queries/`, named `get-daily-txns.sql`:
    ```sql
    -- ~/.cx/queries/get-daily-txns.sql
    SELECT *
    FROM WyndhamTransactionsReport
    WHERE booking_date >= :start_date AND booking_date < :end_date;
    ```
2.  A saved `.transformer.yaml` file in `~/.cx/flows/` (or any other location), named `build-report-artifacts.transformer.yaml`:

    ```yaml
    # ~/.cx/flows/build-report-artifacts.transformer.yaml
    name: "Build Daily Report Artifacts"
    steps:
      - name: "Clean and Prepare Data"
        engine: "pandas"
        operations:
          - type: "rename_columns"
            style: "title_case"
          - type: "convert_column_types"
            to_naive_utc_datetimes: ["Booking Date", "Transaction Timestamp"]

      - name: "Save Formatted Excel Report"
        engine: "file_format"
        operation:
          type: "save"
          format: "excel"
          target_path: "./outputs/daily_report_{{ query_parameters.start_date }}.xlsx"
          artifact_type: "attachment"
          excel_formatting:
            table_style: "TableStyleMedium2"
    ```

3.  An active database connection with the alias `db`.
    ```
    cx> connect user:prod-db --as db
    ```

---

### The Workflow in the Shell

Here is the entire workflow, executed as a single, powerful pipeline and assigned to a variable.

```

cx> report = query run --on db get-daily-txns start_date=2025-09-01 end_date=2025-09-02 | transform run --script ~/.cx/flows/build-report-artifacts.transformer.yaml

```

Let's break down this command:

1.  **`query run --on db ...`**: This executes our saved SQL query against the `db` connection. It passes the `start_date` and `end_date` as parameters, which SQLAlchemy safely binds to the `:start_date` and `:end_date` markers in the SQL file. The result is a JSON object: `{"parameters": {...}, "data": [...]}`.
2.  **`|`**: The pipe sends this JSON object to the next stage.
3.  **`transform run --script ...`**: This executes our transformer workflow.
    - The `TransformerService` intelligently unpacks the `data` array from the piped input.
    - It renames the columns to "Title Case" and formats the dates.
    - It then saves the cleaned data to a dynamically named Excel file (e.g., `daily_report_2025-09-01.xlsx`) inside a newly created `./outputs` directory.
    - The service returns an "Artifact Manifest" as its final result.
4.  **`report = ...`**: The final Artifact Manifest is saved to the `report` variable.

### Inspecting the Result

Now, you can inspect the `report` variable to find the path to your generated Excel file.

```

cx> inspect report

```

The output will show a dictionary containing the path to the Excel file, which you can then open directly:

```

cx> open {{ report.artifacts.attachments[0] }}

```

This recipe demonstrates how `cx` can orchestrate a complex, multi-stage process involving data extraction, transformation, and artifact generation in a single, readable command line. This pipeline can now be saved as its own `.flow.yaml` file for single-command execution in the future.
