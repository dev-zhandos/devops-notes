# Kubernetes Cheat Sheet

Kubernetes (k8s) is a powerful open-source platform for automating the deployment, scaling, and management of containerized applications.

## Table of Contents

- Basic Concepts
- kubectl Commands
- Resource Management
- Networking
- Storage
- Configuration
- Troubleshooting
- Tips and Best Practices
- Understanding Kubernetes YAML File Structure

## Basic Concepts

- **Master Node:** The control plane responsible for cluster management.
- **Worker Node:** Where your application containers run.
- **Pod:** The smallest deployable unit, containing one or more containers.
- **Deployment:** Manages replica sets and provides declarative updates for Pods and ReplicaSets.
- **Service:** Exposes Pods to other Pods or external traffic.
- **Namespace:**  A virtual cluster within your Kubernetes cluster.
- **ConfigMap:** Stores non-confidential configuration data.
- **Secret:** Stores sensitive data like passwords and keys.


## kubectl Commands

| Command                  | Description                                  | Example                                               |
| ------------------------- | -------------------------------------------- | ----------------------------------------------------- |
| `kubectl get`            | List resources                                | `kubectl get pods`, `kubectl get nodes`             |
| `kubectl describe`       | Show detailed information about a resource   | `kubectl describe pod my-pod`                      |
| `kubectl create`         | Create a resource from a file or stdin       | `kubectl create -f my-deployment.yaml`             |
| `kubectl apply`          | Create or update a resource from a file      | `kubectl apply -f my-service.yaml`                |
| `kubectl delete`         | Delete resources                             | `kubectl delete pod my-pod`                       |
| `kubectl logs`           | Fetch the logs of a Pod                      | `kubectl logs my-pod`                             |
| `kubectl exec`           | Execute a command in a container            | `kubectl exec my-pod -c my-container -- ls /`      |
| `kubectl port-forward`   | Forward one or more local ports to a Pod     | `kubectl port-forward my-pod 8080:80`             |
| `kubectl config`         | Manage kubectl configuration files           | `kubectl config view`                              |
| `kubectl cluster-info`   | Display cluster information                  | `kubectl cluster-info`                             |

## Resource Management

- **Pods:** `kubectl create -f pod.yaml`, `kubectl apply -f pod.yaml` 
- **Deployments:** `kubectl create -f deployment.yaml`, `kubectl rollout status deployment my-deployment` (for status)
- **ReplicaSets:**  `kubectl scale replicaset my-replicaset --replicas=3`
- **Services:** `kubectl expose deployment my-deployment --type=LoadBalancer --port=80`
- **Namespaces:** `kubectl create namespace my-namespace`

## Networking

- **Services:** ClusterIP, NodePort, LoadBalancer, ExternalName
- **Ingress:** `kubectl apply -f ingress.yaml`
- **Network Policies:** Control traffic flow between Pods.

## Storage

- **Persistent Volumes (PV):**  Cluster-wide storage resources.
- **Persistent Volume Claims (PVC):** Requests for storage by Pods.
- **Storage Classes:** Provide a way for administrators to describe the "classes" of storage they offer.


## Configuration

- **ConfigMaps:** `kubectl create configmap my-config --from-file=config.properties`
- **Secrets:** `kubectl create secret generic my-secret --from-literal=username=admin --from-literal=password=mypassword`

## Troubleshooting

- **Events:** `kubectl get events`
- **Describe Resources:** `kubectl describe pod my-pod`
- **Logs:** `kubectl logs my-pod`
- **Exec into Pods:** `kubectl exec my-pod -c my-container -- bash` 


## Tips and Best Practices

- Use labels and selectors effectively.
- Design for resilience and scalability.
- Monitor your cluster (e.g., with Prometheus and Grafana).
- Use Helm for package management.

## Understanding Kubernetes YAML File Structure

Kubernetes uses YAML (YAML Ain't Markup Language) files to define the desired state of objects within a cluster. These files are the blueprints for creating and managing various Kubernetes resources, such as Pods, Deployments, Services, and more.

### Key Components of a Kubernetes YAML File

1. **apiVersion:**
   - Specifies the Kubernetes API version to use. This ensures compatibility with the desired Kubernetes features.
   - Example: `apiVersion: apps/v1` (for Deployments in version 1 of the apps API)

2. **kind:**
   - Declares the type of Kubernetes object you're creating (e.g., Pod, Deployment, Service).
   - Example: `kind: Deployment`

3. **metadata:**
   - Provides identifying information about the object.
   - **name:** A unique name for the object within its namespace.
   - **namespace:** (Optional) The namespace where the object belongs. If omitted, it defaults to the "default" namespace.
   - **labels:** Key-value pairs for organizing and selecting objects.
   - Example:

     ```yaml
     metadata:
       name: my-nginx-deployment
       namespace: web-servers
       labels:
         app: nginx
         tier: frontend
     ```

4. **spec:**
   - Defines the desired state or configuration of the object. The contents of `spec` vary greatly depending on the `kind` of object.
   - Example (for a Deployment):

     ```yaml
     spec:
       replicas: 3
       selector:
         matchLabels:
           app: nginx
       template:
         metadata:
           labels:
             app: nginx
         spec:
           containers:
           - name: nginx
             image: nginx:1.14.2
             ports:
             - containerPort: 80
     ```

### Example: NGINX Deployment YAML

```yaml
apiVersion: apps/v1          # Kubernetes version you're using
kind: Deployment            # Type of Kubernetes object (Pod, Deployment, Service, etc.)
metadata:
  name: my-web-app          # A unique name for your object
  namespace: default        # Where your object lives (think of it like a folder)
  labels:                   # Tags for organization and grouping
    app: my-web-app
spec:                        # This is where you set your object's desired state
  replicas: 3               # Number of copies you want running
  selector:                 # Which Pods this Deployment controls
    matchLabels:
      app: my-web-app
  template:                 # How the Pods should look
    metadata:
      labels:
        app: my-web-app
    spec:                   # Pod configuration (containers, volumes, etc.)
      containers:
      - name: my-web-app    
        image: nginx        # Image to use for the container
        ports:
        - containerPort: 80
```
**Explanation:**
- Creates 3 replicas of Nginx pods
- Selects pods with the label `app: nginx`
- Each pod runs the `nginx:1.14.2` container
- Exposes port 80 within the container


### Best Practices for Writing YAML Files

- **Indentation:** Use spaces (not tabs) for indentation, typically two spaces per level.
- **Comments:** Add comments using `#` to explain sections or provide context.
- **Validation:** Use tools like `kubectl apply --dry-run=client -f your-file.yaml` to validate your YAML before applying it to the cluster.
- **Templates:** Leverage Kubernetes Helm to create reusable templates for complex YAML structures.

* **apiVersion:**  Tells Kubernetes which version of its API you're talking to.
* **kind:** The type of object you're creating (like a Pod, Deployment, or Service).
* **metadata:**
    * **name:**  A unique ID for your object in its namespace.
    * **namespace:**  Like a folder to keep things organized. Defaults to `default`.
    * **labels:**  Tags to help you group and manage your objects.
* **spec:** The nitty-gritty details about what you want your object to do.

### Beyond the Basics: More YAML Fun

* **Selectors:**  Let you pick which Pods to manage or send traffic to.
* **Controllers:**  Supervise your Pods, making sure the right number are running and handling updates.
* **Volumes:**  Define how storage (either temporary or persistent) is attached to your containers.
* **Environment Variables:**  Set variables within your containers.
* **Liveness & Readiness Probes:**  Check if your container is alive and kicking or ready to receive traffic.

### YAML Tools and Tips

* **Helm:**  A package manager that makes installing complex Kubernetes apps a breeze using templates.
* **Kustomize:**  Customize your YAML files without editing them directly.
* **YAML Linting:**  Use a linter to catch errors and keep your files tidy.
* **Version Control:**  Track changes to your YAML files with Git (or your favorite version control system) so you can easily undo mistakes.

