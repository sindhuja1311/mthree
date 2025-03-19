# How to Import a Dashboard in Grafana

## Steps to Import a Dashboard

1. Navigate to the dashboard creation interface by clicking the `+` button and selecting `Import Dashboard`.
2. In the import interface, paste the provided JSON code containing the dashboard configuration.
3. After pasting, click the `Load` button to process the configuration.
4. The dashboard will render with all configured panels and visualizations.

## Learning Prometheus and Loki Queries for Grafana
# Prometheus and Loki Basics for Grafana

## Prometheus Basics

- **Purpose**: A powerful time-series database specifically designed for monitoring and alerting in modern cloud-native environments.
- **Data Collection**: Implements a pull-based model, actively scraping metrics from HTTP endpoints at configured intervals.
- **Storage**: Utilizes a highly efficient custom TSDB (Time Series Database) optimized for time-series data storage and retrieval.
- **Querying**: Features PromQL, a flexible query language for complex time-series data analysis.
- **Use Case**: Ideal for comprehensive monitoring of application performance, system resources, and infrastructure metrics.

## Loki Basics

- **Purpose**: A specialized log aggregation system seamlessly integrated with Grafana for unified observability.
- **Data Collection**: Implements an innovative approach by only indexing log labels, not the content itself, for efficient storage.
- **Querying**: Employs LogQL, a powerful query language for log filtering, parsing, and analysis.
- **Use Case**: Perfect for log analysis, debugging, and correlating logs with metrics for comprehensive troubleshooting.

## Differences Between Prometheus and Loki

| Feature           | Prometheus                               | Loki                                      |
|-------------------|------------------------------------------|-------------------------------------------|
| **Data Type**     | Metrics (time-series)                    | Logs                                      |
| **Query Language**| PromQL                                   | LogQL                                     |
| **Storage**       | Custom TSDB                              | Label-based log storage                  |
| **Indexing**      | Indexes all time-series data             | Only indexes labels, not log content     |
| **Use Case**      | Application & infra monitoring           | Log aggregation & debugging              |

### Prometheus Basics

Prometheus serves as a robust time-series database for monitoring and alerting, with PromQL providing powerful querying capabilities.

#### Basic PromQL Concepts:

1. **Metrics and Labels**: 
   - Metrics represent time-series data points
   - Labels provide key-value pairs for categorization
   - Example: `http_requests_total{status="200", method="GET"}`

2. **Key Functions**:
   - `rate()`: Calculates the per-second rate of increase for time-series data
   - `sum()`: Aggregates values across specified dimensions
   - `by()`: Groups results by specific labels for detailed analysis
   - `avg()`, `min()`, `max()`: Performs statistical operations on time-series data

### Loki Basics

Loki specializes in log aggregation with seamless Grafana integration, offering powerful log analysis capabilities through LogQL.

#### Basic LogQL Concepts:

1. **Log Stream Selection**: 
   - Uses labels to identify and filter log streams
   - Example: `{app="nginx", env="production"}`
2. **Log Content Filtering**:
   - Filters logs based on content patterns
   - Example: `{app="nginx"} |= "ERROR"`
3. **Log Parsing and Metrics Extraction**:
   - Utilizes operators like `| pattern` or `| json` for structured data extraction
   - Enables advanced log analysis and metric generation

## Using Prometheus in Grafana

### Step 1: Add Prometheus Data Source

1. Access Grafana's configuration panel through `Configuration > Data Sources`
2. Initiate data source addition by clicking `Add data source`
3. Choose `Prometheus` from the available options
4. Configure the Prometheus server URL (typically `http://localhost:9090`)
5. Validate the connection by clicking `Save & Test`

### Step 2: Create a Dashboard

1. Access the dashboard creation interface via `+ Create > Dashboard`
2. Add visualization panels by clicking `Add new panel`

### Step 3: Write Prometheus Queries

1. Select `Prometheus` as the data source in the panel configuration
2. Input your PromQL query in the query editor:
   ```promql
   rate(http_requests_total{job="api-server"}[5m])
   ```
3. Explore available metrics using the built-in "Metrics browser"
4. Enhance queries using the Function dropdown for advanced analysis

## Using Loki in Grafana

### Step 1: Add Loki Data Source

1. Navigate to `Configuration > Data Sources` in Grafana
2. Click `Add data source` to begin configuration
3. Select `Loki` from the data source options
4. Configure the Loki server URL
5. Verify the connection with `Save & Test`

### Step 2: Create a Logs Panel

1. Add a new panel to your dashboard
2. Select `Logs` as the visualization type

### Step 3: Write Loki Queries

1. Choose `Loki` as the data source in the panel
2. Enter your LogQL query in the query editor:
   ```logql
   {app="nginx"} |= "ERROR" | json
   ```
3. Explore available log streams using the "Log browser"

## Common Prometheus Query Examples

1. **CPU Usage**:
   ```promql
   100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
   ```
2. **Memory Usage**:
   ```promql
   (node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes * 100
   ```
3. **HTTP Error Rate**:
   ```promql
   sum(rate(http_requests_total{status=~"5.."}[5m])) / sum(rate(http_requests_total[5m])) * 100
   ```
4. **Request Latency (99th percentile)**:
   ```promql
   histogram_quantile(0.99, sum(rate(http_request_duration_seconds_bucket[5m])) by (le))
   ```

## Common Loki Query Examples

1. **Filter for Error Logs**:
   ```logql
   {app="myapp"} |= "ERROR"
   ```
2. **Count Error Logs by Service**:
   ```logql
   sum by(service) (count_over_time({app="myapp"} |= "ERROR" [5m]))
   ```
3. **Extract and Count Status Codes**:
   ```logql
   {app="nginx"} | pattern <_> - - <_> "<_> <_> <_>" <status> <_> | count_over_time([5m]) by (status)
   ```
4. **Parse JSON Logs**:
   ```logql
   {app="api"} | json | response_time > 200
   ```

I've practiced these examples extensively in my Grafana instance to develop proficiency with both query languages. The learning approach involved starting with basic queries and progressively incorporating more complex patterns as familiarity grew.

![Image](https://github.com/user-attachments/assets/dce305a2-258f-49c1-9e9b-d95d385d1538)

## Steps to Import a Dashboard

1. Access the dashboard import interface by clicking the `+` button and selecting `Import Dashboard`
2. Insert the dashboard configuration by pasting the provided JSON code
3. Process the configuration by clicking the `Load` button
4. The dashboard will render with all configured panels and visualizations

![Image](https://github.com/user-attachments/assets/4e618502-479b-440e-8de8-ead69d537b2d)

## Steps to Export a Dashboard

1. Open the target dashboard in Grafana
2. Locate and click the Share icon in the top-right corner of the interface
3. Navigate to the Export tab in the sharing dialog
4. Click `Save JSON` to download the dashboard configuration as a JSON file

This exported JSON file can be shared with team members or imported into other Grafana instances for consistent dashboard deployment.