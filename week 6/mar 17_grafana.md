
GRAFANA

## Toil:
In the context of Site Reliability Engineering (SRE), toil encompasses all operational tasks that share these key characteristics:
- Must be manual and repetitive
- Can be automated but isn't yet
- Provides no lasting service value
- Directly scales with service growth
- Takes time away from strategic engineering work that could improve system reliability, scalability, and performance

### SREs Handle Toil:
1. **Automate** – Transform repetitive manual work into automated processes through scripts and pipelines
2. **Eliminate** – Address root causes to permanently remove sources of toil
3. **Reduce** – Optimize and streamline necessary manual processes
4. **Limit Toil** – Following Google's SRE best practice: keep toil under 50% of team capacity to ensure focus on strategic improvements

## Prometheus
- A sophisticated open-source monitoring and alerting toolkit
- Employs a pull-based architecture to collect metrics at configurable intervals
- Stores data in a powerful time-series database
- Excels at gathering system metrics including CPU, memory, network statistics, and custom application metrics

## Grafana
- Industry-leading open-source visualization and analytics platform
- Creates dynamic, real-time dashboards from Prometheus metrics
- Features comprehensive data source compatibility (Prometheus, Loki, InfluxDB, MySQL, Elasticsearch, etc.)
- Enables real-time system health monitoring through intuitive dashboards

## Loki
- Next-generation log aggregation system
- Implements Prometheus-like approach to log management
- Innovates by indexing metadata rather than full log content
- Seamlessly integrates logs with Prometheus metrics for comprehensive debugging

## Running the Grafana Dashboard Script
1. Firstly, it asks whether to reset the minikube or not. If we select yes then it stops and deletes the existing minikube cluster.
2. In the script, we have mentioned to start minikube:
```bash
minikube start --driver=docker --cpus=2 --memory=3072
```
3. Next created a `sample-app.yaml` file, where it is having a container with three types of messages:
   - **[INFO]** – Regular messages
   - **[DEBUG]** – Debugging messages
   - **[ERROR]** – Random error messages
4. Next, we need to install Prometheus, but before that delete the namespace if any exists.
5. Installing Prometheus through Helm.

### Helm
- The essential package manager for Kubernetes environments
- Simplifies deployment and management of Kubernetes applications
- Utilizes Helm charts for streamlined application lifecycle management
- Particularly valuable for complex deployments like Prometheus, Grafana, and Loki

6. There is a file namely `values.yaml`, where it is used to adjust the settings of Prometheus. Helm reads the `values.yaml` file and sets up Prometheus with the specified settings.
7. After that, waiting for Prometheus to start using the code:
```bash
kubectl wait --for=condition=ready pod --selector="app.kubernetes.io/name=prometheus,app.kubernetes.io/component=server" -n monitoring --timeout=120s
```
8. It ensures that Prometheus is ready to go to the next step.
9. Next, we installed Loki-stack from the Grafana chart.
10. We have created `values.yaml` file, it consists of:
   - admin, password
   - datasources including URL
   - dashboard providers and preload it
11. Installing the Grafana chart through Helm and providing the `values.yaml` file as well.
12. After that exposing Grafana and forwarding the port.
13. We have mentioned the custom dashboard using JSON format, but we haven't used it. We created dashboards using UI.
14. After that run the script using the command:
```bash
./simple-grafana-monitoring.sh
```
15. Commands that made me execute this file:
```bash
sudo dockerd &
minikube delete
minikube start --driver=docker --cpus=2 --memory=2048
minikube status
```

output be like:

```bash
sandy@sandy:~$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

```bash
kubectl get nodes
```

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod +x get_helm.sh
./get_helm.sh
```

16. Next deleted the kubectl monitoring namespace.
17. Finally executed the file again.

## Creating the Grafana Dashboard
1. Open Grafana using the exposed URL.
2. Login with username and password.
3. Create a new dashboard in Grafana.
4. Select **Loki** as a data source.
5. From the visualization option, select **"Logs"**.
6. Set the panel title and click on apply.
7. In label filters, select **namespace** and give **sample-app** there. In **Line Contains** tab, give the word **ERROR**.

## Creating CPU Usage Panel
1. Select **Prometheus** as a data source.
2. Switch from builder to code mode.
3. Execute the below query:
```promql
sum(rate(container_cpu_usage_seconds_total{namespace="sample-app"}[5m])) by (pod)
```
4. The query returns the average CPU usage rate (in seconds per second) for each pod within the sample-app namespace over the last 5 minutes.
5. Set the unit to **Percent (0-100)**.

## Creating Log Volume Panel
1. Created another panel in the same dashboard called **Log Volume by Pod**.
2. Executed the query below:
```promql
sum(count_over_time({namespace="sample-app"}[5m])) by (pod_name)
```
3. Configured dashboard settings and saved the dashboard.
4. Dashboard now shows 3 panels.

## Monitoring Stack Components

### Prometheus: Time-Series Monitoring
Prometheus serves as the foundation of modern monitoring systems:
- Purpose-built for cloud-native environments
- Pull-based metrics collection architecture
- Powerful time-series database
- Key features:
  - Automated service discovery
  - Multi-dimensional data model
  - Sophisticated query language (PromQL)
  - Built-in alerting capabilities

### Grafana: Visualization Platform
Grafana transforms complex metrics into actionable insights:
- Advanced visualization capabilities
- Multi-source data integration
- Real-time monitoring features
- Key strengths:
  - Interactive dashboards
  - Extensive data source support
  - Alert management
  - Team collaboration features

### Loki: Log Aggregation System
Loki revolutionizes log management with its unique approach:
- Designed for cost-effective log aggregation
- Prometheus-inspired label-based indexing
- Efficient storage architecture
- Notable features:
  - Label-based log querying
  - Integration with Prometheus metrics
  - Grafana native support
  - Resource-efficient design

## Implementing the Monitoring Stack

### Setting Up the Environment
1. Firstly, it asks whether to reset the minikube or not. If we select yes then it stops and deletes the existing minikube cluster.
2. In the script, we have mentioned to start minikube:
```bash
minikube start --driver=docker --cpus=2 --memory=3072
```
3. Next created a `sample-app.yaml` file, where it is having a container with three types of messages:
   - **[INFO]** – Regular messages
   - **[DEBUG]** – Debugging messages
   - **[ERROR]** – Random error messages
4. Next, we need to install Prometheus, but before that delete the namespace if any exists.
5. Installing Prometheus through Helm.

### Helm: Kubernetes Package Management
Helm streamlines Kubernetes application deployment:
- Standardized package management
- Template-based releases
- Version control for deployments
- Benefits:
  - Simplified application installation
  - Consistent deployment patterns
  - Easy updates and rollbacks
  - Managed configuration

### Configuration and Deployment Steps
1. **Initial Setup**
   - Environment preparation
   - Minikube cluster initialization
   - Resource allocation optimization

2. **Sample Application Deployment**
   - Log generation configuration
   - Message type implementation:
     - INFO: Standard operational logs
     - DEBUG: Detailed troubleshooting data
     - ERROR: Critical issue tracking

3. **Prometheus Installation**
   - Namespace management
   - Helm chart deployment
   - Custom configuration implementation

4. **Monitoring Stack Configuration**
   - Values file customization
   - Data source integration
   - Dashboard provisioning

6. There is a file namely `values.yaml`, where it is used to adjust the settings of Prometheus. Helm reads the `

### Monitoring Stack Deployment
1. **Prometheus Configuration**
   - Configure values.yaml for Prometheus settings
   - Deploy using Helm chart
   - Verify pod readiness:
   ```bash
   kubectl wait --for=condition=ready pod --selector="app.kubernetes.io/name=prometheus,app.kubernetes.io/component=server" -n monitoring --timeout=120s
   ```

2. **Loki Integration**
   - Deploy Loki stack via Grafana chart
   - Configure values.yaml with:
     - Authentication credentials
     - Data source configurations
     - Dashboard provisioning settings

3. **Grafana Setup**
   - Deploy via Helm with custom values
   - Configure port forwarding
   - Enable external access

### Deployment Verification
```bash
sudo dockerd &
minikube delete
minikube start --driver=docker --cpus=2 --memory=2048
minikube status
```

Output verification:
```bash
sandy@sandy:~$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

### Dashboard Configuration Guide

#### Basic Dashboard Setup
1. Access Grafana interface
2. Authenticate with credentials
3. Initialize new dashboard
4. Configure Loki data source
5. Select visualization type
6. Configure panel settings
7. Implement log filtering:
   - Namespace: sample-app
   - Filter: ERROR messages

#### Advanced Metrics Configuration

##### CPU Monitoring Panel
1. Configure Prometheus data source
2. Enable code mode
3. Implement CPU metrics:
```promql
sum(rate(container_cpu_usage_seconds_total{namespace="sample-app"}[5m])) by (pod)
```
4. Configure for percentage display (0-100)

##### Log Analytics Panel
1. Create "Log Volume by Pod" visualization
2. Implement log volume metrics:
```promql
sum(count_over_time({namespace="sample-app"}[5m])) by (pod_name)
```
3. Apply dashboard optimizations
4. Verify multi-panel integration