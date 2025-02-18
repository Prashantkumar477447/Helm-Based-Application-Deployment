# Helm-Based-Application-Deployment
**Helm-Based Deployment Repo** 🚀   This repo contains Helm charts for deploying applications on Kubernetes. It includes templates, values.yaml, and automation scripts for easy and scalable deployments. Perfect for learning and managing Kubernetes apps efficiently.

# Helm-Based Deployment for Node.js Application

## Section 1: Introduction and Helm Basics

### What is Helm?
Helm is often referred to as the **package manager for Kubernetes**. It enables you to define, install, and manage even the most complex Kubernetes applications. Helm uses a packaging format called **charts**, which include all the resources needed to run an application, service, or a complete cloud-native stack inside Kubernetes.

### How to Install Helm in Ubuntu
```sh
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

### Important Helm Commands
- `helm create [CHART]` - Scaffold a new Helm chart.
- `helm package [CHART]` - Package the chart into a chart archive.
- `helm install [NAME] [CHART]` - Install a Helm chart.
- `helm upgrade [NAME] [CHART]` - Upgrade an installed Helm chart.
- `helm uninstall [NAME]` - Uninstall an installed Helm chart.
- `helm list` - List all installed Helm charts.
- `helm rollback [NAME] [REVISION]` - Roll back a release to a specific revision.

## Section 2: Prerequisites
- Helm installed
- Kubernetes cluster set up (e.g., Minikube, Kind, or any cloud-based Kubernetes)
- Docker installed (Optional for custom images)
- Basic understanding of Kubernetes resources like Pod, Service, Deployment

## Section 3: Create Helm Chart Structure
Run the following command to scaffold a new Helm chart:
```sh
helm create node-app-chart
```
This will create a folder named **node-app-chart** with the initial chart structure.

## Section 4: Chart Metadata (Chart.yaml)
Modify **Chart.yaml** to include your application details:
```yaml
apiVersion: v2
name: node-app-chart
description: A Helm chart for a Node.js application
version: 0.1.0
```

## Section 5: Default Values (values.yaml)
Edit **values.yaml** to include:
```yaml
replicaCount: 1

image:
  repository: trainwithshubham/node-app-test-new
  tag: latest
  pullPolicy: IfNotPresent

service:
  type: NodePort
  port: 30007
  targetPort: 8000
```

## Section 6: Deployment Manifest (templates/deployment.yaml)
Modify **deployment.yaml** under **templates/** to look like this:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-deployment
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Release.Name }}
  template:
    metadata:
      labels:
        app: {{ .Release.Name }}
    spec:
      containers:
      - name: {{ .Chart.Name }}
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
        imagePullPolicy: {{ .Values.image.pullPolicy }}
        ports:
        - containerPort: 8000
```

## Section 7: Service Manifest (templates/service.yaml)
Modify **service.yaml** under **templates/**:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}-service
spec:
  type: {{ .Values.service.type }}
  ports:
  - port: {{ .Values.service.port }}
    targetPort: {{ .Values.service.targetPort }}
    nodePort: {{ .Values.service.port }}
  selector:
    app: {{ .Release.Name }}
```

## Section 8: Deploying the Helm Chart
Use the following commands to package and deploy the chart:
```sh
# Package the chart (Optional)
helm package node-app-chart

# Deploy the chart
helm install my-node-app ./node-app-chart
```

## Section 9: List the Deployment
```sh
# List installed Helm charts
helm list

# Check the status of the deployed chart
helm status my-node-app
```

## Conclusion
This guide provides a step-by-step approach to deploying a Node.js application using Helm on Kubernetes. With Helm, you can simplify Kubernetes deployments and manage application updates efficiently. 🚀

