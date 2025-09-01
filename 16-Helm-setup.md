# Helm Fundamentals

- Introduction and Helm Basics
- What is Helm?
  Helm is often referred to as the package manager for Kubernetes. It enables you to define, install, and manage even the most complex Kubernetes applications. Helm uses a packaging format called charts, which include all the resources needed to run an application, service, or a complete cloud-native stack inside Kubernetes.
  
  # Prerequisites
   - Docker installed (Optional for custom images) 
   - Kubernetes cluster set up (e.g., Minikube, kind, or any cloud-based Kubernetes) 
   - kubectl installed and configured.
   - Helm 3 installed. Install Helm with:

- How to Install helm in Ubuntu
```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```
- or using script
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

- Chart Structure
```
apache/
├── Chart.yaml         # Chart metadata
├── values.yaml        # Default values for customization
└── templates/         # Kubernetes resource templates
    ├── deployment.yaml
    └── service.yaml
```

- Important Helm Commands
```
helm create [CHART]: Scaffold a new Helm chart.
helm package [CHART]: Package the chart into a chart archive.
helm install [NAME] [CHART]: Install a Helm chart.
helm upgrade [NAME] [CHART]: Upgrade an installed Helm chart.
helm uninstall [NAME]: Uninstall an installed Helm chart.
helm list: List all installed Helm charts.
helm rollback [NAME] [REVISION]: Roll back a release to a specific revision.
```
## Example of Apache

Install the Helm chart:
```bash

helm install apache ./apache --namespace apache-namespace --create-namespace
```
Default Configuration
The chart deploys an Apache HTTP Server with the following default values (from values.yaml):

```yaml

replicaCount: 2

image:
  repository: httpd
  tag: 2.4
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80

resources:
  requests:
    memory: "64Mi"
    cpu: "100m"
  limits:
    memory: "128Mi"
    cpu: "200m"
```
You can customize these values by modifying values.yaml or passing them via the --set flag during installation.

Accessing the Deployment
Check the status of your pods and services:
```bash

kubectl get pods -n apache-namespace
kubectl get svc -n apache-namespace
```
Forward the service port to access Apache locally:
```bash

kubectl port-forward svc/apache 8080:80 -n apache-namespace
```
Open your browser and visit:
```arduino

http://localhost:8080
```
Uninstallation
To remove the deployment and associated resources:

```bash

helm uninstall apache -n apache-namespace
kubectl delete namespace apache-namespace
```
Customizing the Chart
You can override default values using the --set flag. For example:

```bash

helm install apache ./apache \
  --namespace apache-namespace \
  --set replicaCount=3 \
  --set image.tag=2.4.53 \
  --set service.type=LoadBalancer
```
Alternatively, modify the values.yaml file directly.

### Features
- Scalable: Set replicaCount to scale the number of pods.
- Configurable Resources: Customize CPU/memory requests and limits.
- Customizable Service: Supports ClusterIP, NodePort, or LoadBalancer service types.

### Troubleshooting

Verify Helm and Kubernetes versions:
```bash

helm version
kubectl version
```
Check Helm release status:
```bash

helm status apache -n apache-namespace
```
Inspect pod logs:
```bash

kubectl logs <apache-pod-name> -n apache-namespace

```
