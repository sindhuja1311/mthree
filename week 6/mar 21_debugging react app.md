## Updates to `complete-react-sre-script.sh`

### Modification 1
- **Previous Line:**
  ```bash
  if ! grep -q Microsoft /proc/version; then
  ```
- **Revised Line:**
  ```bash
  if ! grep -qEi "(microsoft|wsl)" /proc/version; then
  ```
- **Approximate Line Number:** 82 (Confirm within the actual file)

**Enhancement:** This update enhances the WSL detection mechanism by making it case-insensitive and explicitly accounting for variations like "wsl."

---

## Updates to `deploy_app.py`

### Modification 2
- **Original Function:** `create_namespaces`
  ```python
  def create_namespaces():
      """Create necessary Kubernetes namespaces."""
      log("INFO", "Creating Kubernetes namespaces...")
  
      namespaces = ["react-sre-app", "monitoring"]
      for namespace in namespaces:
          result = run_command([
              KUBECTL_CMD, "create", "namespace", namespace, "--dry-run=client", "-o", "yaml"
          ], timeout=10)
  
          if result is None:
              log("ERROR", f"Failed to generate namespace YAML for {namespace}")
              return False
  
          apply_result = run_command(f"{KUBECTL_CMD} apply -f -", timeout=10, shell=True, retry=3)
  
          if apply_result is None:
              log("ERROR", f"Failed to create namespace {namespace}")
              return False
  
      log("INFO", "Kubernetes namespaces created successfully")
      return True
  ```
- **Updated Function:**
  ```python
  def create_namespaces():
      """Ensure required Kubernetes namespaces exist."""
      log("INFO", "Verifying the existence of required Kubernetes namespaces...")
  
      namespaces = ["react-sre-app", "monitoring"]
      for namespace in namespaces:
          if namespace_exists(namespace):
              log("INFO", f"Namespace '{namespace}' is already present. Skipping creation.")
              continue
  
          log("INFO", f"Creating namespace: {namespace}")
  
          create_result = run_command(
              [KUBECTL_CMD, "create", "namespace", namespace],
              timeout=10,
              retry=3
          )
  
          if create_result is None:
              log("ERROR", f"Failed to create namespace '{namespace}'")
              return False
  
      log("INFO", "All necessary namespaces are now ready.")
      return True
  ```
- **Approximate Line Number:** 285 (Verify in the file)

### Modification 3
- **New Function:** `namespace_exists`
  ```python
  def namespace_exists(namespace):
      """Check if a Kubernetes namespace is already present."""
      result = run_command(
          [KUBECTL_CMD, "get", "namespace", namespace, "-o", "jsonpath={.metadata.name}"],
          timeout=10
      )
      return result is not None and namespace in result
  ```
- **Approximate Line Number:** 275 (Verify in the file)

**Enhancement:** Introduced a `namespace_exists` function to prevent redundant namespace creation, optimizing efficiency.

### Modification 4
- **Updated Function:** `run_command`
  - **New Parameter Added:** `cwd=None`
  - **Enhancement:** Included `cwd=cwd` in `subprocess.run` to allow specifying a working directory.
  - **Approximate Line Number:** 120 (Verify in the file)

---

## Updates to `nginx.conf`

### Modification 5
- **Enhanced Configuration:** Modified `/health` and `/metrics` endpoints to improve response handling.
- **File:** `nginx.conf`

![image](https://github.com/user-attachments/assets/987c5cfa-a906-4644-ba99-09f1f9009cd7)

---

## Updates to `deployment.yaml`

### Modification 6
- **Refined Configuration:** Updated deployment settings, Prometheus annotations, ports, and container image.
- **File:** `deployment.yaml`

![image](https://github.com/user-attachments/assets/d63281d1-7ac2-4533-b4e7-54e9025883b8)
![image](https://github.com/user-attachments/assets/4fcf444e-3f42-481f-920a-6adec9855027)
![image](https://github.com/user-attachments/assets/c49433b7-013f-45ba-84b0-e747c2328370)

---

## Prometheus Setup

![image](https://github.com/user-attachments/assets/16c95e5e-40bb-4b3d-af00-46de07a30617)
![image](https://github.com/user-attachments/assets/5ae7f8c4-aaf7-4d38-9785-a7332e4e5e57)

## Grafana Setup

![image](https://github.com/user-attachments/assets/e63fe0d3-af3f-4e96-aae2-3911241782df)

---

## Deployment Steps

After making these modifications, execute the following commands to apply them:
```bash
sudo systemctl start docker || sudo service docker start
minikube delete
minikube start
./deploy_sre_app.sh
```

