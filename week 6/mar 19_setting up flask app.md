# Grafana Dashboards and SRE Concepts

## Latency – Response Time Measurement

- **Real-world Analogy:** Consider a restaurant order - latency represents the entire journey from order placement to food delivery.
- **Technical Definition:** The time interval between a user's request and the system's response.
  - High latency directly impacts user satisfaction and system performance.
  - Critical for real-time applications where milliseconds matter.

## Traffic – System Load Measurement

- **Real-world Analogy:** Think of a traffic counter at a store entrance - it tracks customer flow.
- **Technical Definition:** The volume of requests processed by the system per unit time.
  - Essential for capacity planning and resource allocation.
  - Helps identify peak usage patterns and potential bottlenecks.

## Errors – System Reliability Indicator

- **Real-world Analogy:** Similar to quality control in manufacturing - tracking defective products.
- **Technical Definition:** Monitoring system failures and error conditions.
  - Critical for maintaining system reliability and user trust.
  - Helps identify patterns in system failures and areas for improvement.

## Saturation – Resource Utilization

- **Real-world Analogy:** Like a restaurant's seating capacity - how full the system is.
- **Technical Definition:** Measuring system resource utilization and capacity limits.
  - Key indicator of system health and performance.
  - Helps prevent system overload and maintain optimal performance.

# Observability

## Metrics – Performance Indicators

- **Real-world Analogy:** Similar to a car's dashboard - showing speed, fuel, and engine status.
- **Technical Definition:** Quantitative measurements of system performance.
  - Provides real-time insights into system behavior.
  - Enables proactive performance optimization.

## Logs – System Activity Record

- **Real-world Analogy:** Like a detailed flight recorder - capturing every system event.
- **Technical Definition:** Comprehensive record of system activities and events.
  - Essential for debugging and system analysis.
  - Enables historical analysis and pattern recognition.

## Visualization – Data Interpretation

- **Real-world Analogy:** Similar to a control room dashboard - presenting complex data clearly.
- **Technical Definition:** Graphical representation of system metrics and logs.
  - Makes complex data easily understandable.
  - Enables quick decision-making and problem identification.

## Instrumentation – System Monitoring

- **Real-world Analogy:** Like medical monitoring equipment - tracking vital signs.
- **Technical Definition:** Implementation of monitoring and measurement systems.
  - Provides detailed insights into system behavior.
  - Enables proactive system maintenance.

# Health Monitoring

## Liveness Probes – Basic System Check

- **Real-world Analogy:** Similar to a heartbeat monitor - checking if the system is operational.
- **Technical Definition:** Basic health check ensuring system availability.
  - Critical for system reliability monitoring.
  - Helps identify system failures quickly.

## Readiness Probes – System Capability Check

- **Real-world Analogy:** Like a pre-flight checklist - ensuring all systems are ready.
- **Technical Definition:** Verification of system readiness to handle requests.
  - Ensures system can process requests properly.
  - Prevents premature traffic routing.

## Health Endpoints – Detailed System Status

- **Real-world Analogy:** Similar to a comprehensive medical checkup.
- **Technical Definition:** Detailed monitoring of various system components.
  - Provides comprehensive system health overview.
  - Enables targeted problem resolution.

## Dependency Checks – System Integration

- **Real-world Analogy:** Like checking all components of a complex machine.
- **Technical Definition:** Verification of system dependencies and connections.
  - Ensures all system components work together.
  - Prevents cascading failures.

# Service Level Objectives

## Error Rate Tracking – Quality Measurement

- **Real-world Analogy:** Similar to quality control metrics in manufacturing.
- **Technical Definition:** Monitoring and analyzing system error rates.
  - Critical for maintaining service quality.
  - Helps set and achieve reliability goals.

## Latency Histograms – Response Time Analysis

- **Real-world Analogy:** Like analyzing delivery time distributions.
- **Technical Definition:** Statistical analysis of system response times.
  - Helps understand system performance patterns.
  - Enables performance optimization.

## Request Success Rate – Reliability Metric

- **Real-world Analogy:** Similar to customer satisfaction ratings.
- **Technical Definition:** Measurement of successful request processing.
  - Key indicator of system reliability.
  - Helps maintain service quality standards.

# Running the Script

- Execute the monitoring script using:
  ```sh
  ./lightweight-sre-monitoring.sh
  ```
- For Docker permission issues, start the Docker daemon:
  ```sh
  sudo dockerd
  ```
- If encountering permission errors, adjust script permissions:
  ```sh
  chmod 777 lightweight-sre-monitoring.sh
  ```
- For `--short` errors, remove the flag from kubectl commands in the script.
- For timeout issues, extend the wait time from `120` to `200` seconds.
- Upon successful execution, access the Grafana dashboard at:
  ```
  http://localhost:3000
  ```
![image](https://github.com/user-attachments/assets/f1e48f70-ff8d-495e-9cba-162b0ffd4863)
<img width="299" alt="image" src="https://github.com/user-attachments/assets/b5f7b4b9-8255-43b2-9ac0-38da1e98d75c" />

# Creating Dashboards and Panels

- **Step 1:** Navigate to Dashboards → Add New Dashboard → Add Visualization
- **Step 2:** Configure Prometheus as the data source
- **Step 3:** Choose appropriate visualization type (Stat, Time Series, Bar Chart)
- **Step 4:** Implement the following queries:

### Panel 1: Request Rate per Endpoint (Stat Visualization)
```promql
sum by(endpoint) (rate(app_request_count_total{namespace="sre-demo"}[24h]))
```

### Panel 2: HTTP 5xx Error Rate (Stat Visualization)
```promql
sum(rate(app_request_count{namespace="sre-demo", http_status=~"5.."}[1m])) / sum(rate(app_request_count{namespace="sre-demo"}[1m])) * 100
```

### Panel 3: Active Requests (Stat Visualization)
```promql
app_active_requests{namespace="sre-demo"}
```

### Panel 4: 95th Percentile Request Latency (Time Series Visualization)
```promql
histogram_quantile(0.95, sum(rate(app_request_latency_seconds_bucket{namespace="sre-demo"}[5m])) by (le, endpoint))
```

### Panel 5: Total Requests Count (Stat Visualization)
```promql
app_request_latency_seconds_count{namespace="sre-demo"}
```

### Panel 6: Kubernetes API Server Cache List Operations (Bar Chart)
```promql
rate(apiserver_cache_list_total{job="kubernetes-apiservers"}[5m])
```

### Panel 7: Total Cache List Operations (Gauge Visualization)
```promql
sum(apiserver_cache_list_total{job="kubernetes-apiservers"})
```

## Prometheus Dashboard Queries

### 1. Total Number of Requests (Counter)
**Query:**
```promql
sum(app_request_count_total)
```
**Visualization Options:**
- Stat Panel: Displays current request count
- Time-Series Graph: Shows request volume trends

### 2. Requests Per Endpoint (Bar Chart)
**Query:**
```promql
sum by (endpoint) (app_request_count_total)
```
**Visualization Options:**
- Bar Chart: Compares endpoint usage
- Heatmap: Shows request patterns

### 3. Request Latency Over Time
**Query:**
```promql
rate(app_request_latency_seconds_sum[5m]) / rate(app_request_latency_seconds_count[5m])
```
**Visualization Options:**
- Time-Series Graph: Tracks latency trends
- Heatmap: Shows latency distribution

### 4. Error Rate (Percentage)
**Query:**
```promql
(sum(app_error_count_total) / sum(app_request_count_total)) * 100
```
**Visualization Options:**
- Gauge: Shows current error rate
- Time-Series Graph: Tracks error trends

### 5. CPU Usage Over Time
**Query:**
```promql
rate(process_cpu_seconds_total[5m])
```
**Visualization Options:**
- Time-Series Graph: Shows CPU utilization
- Area Graph: Highlights usage patterns

### 6. Memory Usage
**Query:**
```promql
process_resident_memory_bytes
```
**Visualization Options:**
- Gauge: Shows current memory usage
- Time-Series Graph: Tracks memory trends

### 7. Active Requests
**Query:**
```promql
app_active_requests
```
**Visualization Options:**
- Gauge: Shows current active requests
- Time-Series Graph: Tracks request patterns

### 8. Requests per HTTP Status Code
**Query:**
```promql
sum by (http_status) (app_request_count_total)
```
**Visualization Options:**
- Pie Chart: Shows status code distribution
- Bar Chart: Compares status code counts

### Saving Dashboard
- Click **Save Dashboard** → Name it `SRE Basic Dashboard`

# Log Analysis using Loki

- **Step 1:** Create a new dashboard and add visualization panels
- **Step 2:** Configure **Loki** as the data source
- **Step 3:** Select visualization type and implement queries:

### Panel 1: Filter Logs for `sre-demo` Namespace (Logs Visualization)
```logql
{namespace="sre-demo"}
```

### Panel 2: Filter Logs for Errors (Stat Visualization)
```logql
{namespace="sre-demo"} |= `Error`
```

### Panel 3: Log Count over Time (Stat Visualization)
```logql
sum(count_over_time({namespace="sre-demo"}[1m]))
```

### Saving Dashboard
- Click **Save Dashboard** → Name it `Log Analysis`
<img width="757" alt="image" src="https://github.com/user-attachments/assets/c6adfe45-b28a-4ecb-9e04-a99933fd167f" />
<img width="758" alt="image" src="https://github.com/user-attachments/assets/94027aa8-b5dc-4b92-8461-c193e0b2229d" />


### Flask-APP
Commands:
## Docker and Kubernetes Commands with Explanation

### 1. Build Docker Image
```sh
docker build -t sre-flask-app:latest .
```
**Explanation:**
- `docker build` → Command to build a Docker image.
- `-t sre-flask-app:latest` → Tags the image with the name `sre-flask-app` and the `latest` tag.
- `.` → Uses the current directory as the build context.

### 2. Restart Kubernetes Deployment
```sh
kubectl rollout restart deployment sre-flask-app -n sre-demo
```
**Explanation:**
- `kubectl rollout restart` → Restarts a Kubernetes deployment.
- `deployment sre-flask-app` → Specifies the deployment name (`sre-flask-app`).
- `-n sre-demo` → Targets the `sre-demo` namespace.

### 3. Port Forward Kubernetes Service
```sh
kubectl port-forward svc/sre-flask-app 8081:80 -n sre-demo
```
**Explanation:**
- `kubectl port-forward` → Forwards a local port to a service in Kubernetes.
- `svc/sre-flask-app` → Specifies the service name (`sre-flask-app`).
- `8081:80` → Maps local port `8081` to container port `80`.
- `-n sre-demo` → Targets the `sre-demo` namespace.

### 4. Configure Docker to Use Minikube's Docker Daemon
```sh
eval $(minikube docker-env)
```
**Explanation:**
- `minikube docker-env` → Outputs environment variables for using Minikube's Docker daemon.
- `eval $(...)` → Evaluates and applies these environment variables to the current shell session.

### 5. Get All Pods in the Monitoring Namespace
```sh
kubectl get pods -n monitoring
```
**Explanation:**
- `kubectl get pods` → Lists all running pods in Kubernetes.
- `-n monitoring` → Filters the results to the `monitoring` namespace.

### 6. Port Forward Prometheus Pushgateway
```sh
kubectl port-forward prometheus-prometheus-pushgateway-544579d549-q8g7g 9091 -n monitoring
```
**Explanation:**
- `kubectl port-forward` → Forwards a local port to a pod in Kubernetes.
- `prometheus-prometheus-pushgateway-544579d549-q8g7g` → Specifies the pod name.
- `9091` → Maps local port `9091` to the container's default port.
- `-n monitoring` → Targets the `monitoring` namespace.


<img width="559" alt="image" src="https://github.com/user-attachments/assets/aebb915f-f260-4e1a-a5fd-ed4e02270d88" />
<img width="547" alt="image" src="https://github.com/user-attachments/assets/ae0324fc-6147-472c-920f-b1e35ff8b37a" />
<img width="493" alt="image" src="https://github.com/user-attachments/assets/259fd2e1-0cc0-4275-996f-14595a72df2d" />

## localhost:8080
<img width="677" alt="image" src="https://github.com/user-attachments/assets/28a208c8-2cf3-427a-af73-ce97994de081" />
![Users]
<img width="550" alt="image" src="https://github.com/user-attachments/assets/db64668f-4203-41b1-978b-da451939b456" />
![Slow]
<img width="310" alt="image" src="https://github.com/user-attachments/assets/cd283341-99aa-474b-8681-759a67ac05ae" />
![Metrics]
<img width="535" alt="image" src="https://github.com/user-attachments/assets/97c963ad-fc31-4d09-8a7e-0eb670b9a248" />
[New dashboard-1742370827591.json](https://github.com/user-attachments/files/19346756/New.dashboard-1742370827591.json)

# Final Steps
- Reviewed all concepts in the afternoon session
- Prepared for the Python exam using LMS