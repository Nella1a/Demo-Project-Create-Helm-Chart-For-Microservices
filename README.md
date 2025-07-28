# Deploy Microservices with Helmfile (on Linode)

## Description

This project demonstrates how to deploy the [Online Boutique demo application](https://github.com/GoogleCloudPlatform/microservices-demo) using Helmfile.

## Prerequisites

- A Linode account with access to a configured and running Kubernetes cluster.[Follow this guide for detailed setup steps](https://github.com/Nella1a/Demo-project-Deploy-Microservices-Application)

### Create Helm Charts

#### Microservices

```bash
helm create <name-of-chart>

```

If the command is successful, a new auto-generated folder named [name-of-chart] will appear in your root directory.
This folder includes subdirectories (charts, templates) and files such as .helmignore, Chart.yaml, and values.yaml.

Basic Structure of a Helm Chart

- Chart.yaml: Contains chart metadata.
- charts/: Holds any dependency charts required by this chart.
- .helmignore: Specifies which files and folders Helm should ignore when packaging.
- templates/: The core of the Helm chart. This is where Kubernetes YAML templates live.
- values.yaml: Stores values that will populate your template files.

Customize Template Files
Create your own templates from scratch:

1. Remove all files inside the templates/ folder except deployment.yaml and service.yaml.

2. Clear the contents of deployment.yaml, service.yaml, and values.yaml.

Define Helm Template Placeholders

- Use placeholder syntax for values:

```yaml
{ { .Values.<variableName> } }
```

For environment variables

```yaml
value: { { .Values.<envVar> | quote } }
```

Set Template Values
Define your variables in the values.yaml file.

Validate Helm Chart

- Locally render templates:

```bash
helm template -f <service-name-values.yaml> <name-of-chart>
```

- Or simulate an install (connects to cluster for validation):

```bash
helm install --dry-run -f <service-name-values.yaml> <release-name> <name-of-chart>
```

helm template only renders locally. --dry-run performs validation with the Kubernetes API.

Lint Helm Chart
Check for syntax issues and best practices:

```bash
helm lint
```

- ERROR: Will block installation.
- WARNING: May break conventions or recommendations.

Install Helm Chart

```bash
    helm install -f <service-name-values.yaml> <release-name> <chart-name>
```

<release-name> is the name of the deployed application in the cluster.

#### Redis (Third-party Application)

Create a chart for Redis:

```bash
helm create redis
```

As before, remove all files in the templates/ folder except deployment.yaml and service.yaml, and build your templates from scratch.

### Deploy Microservices with Helmfile

Option 1: Install Charts Individually Using a Script
Install each service manually or use a script file, e.g., install.sh:

```bash
helm install -f values/<redis-values.yaml> rediscart charts/redis
```

Make the script executable:

```bash
chmod u+x install.sh
```

Run the script:

````bash
./install.sh
```

Option 2: Use Helmfile

- Define the desired state of your cluster in a single YAML file.
- Manage multiple Helm releases with custom values for each environment or application.


Install Helmfile

```bash
brew install helmfile
```



Deploy Using Helmfile

```bash
helmfile sync
```

- Visit your Linode Kubernetes dashboard.
- Use the public IP of the NodeBalancer to access the application:


  ```ccp
  http://<NodeBalancer-public-ip>:80
  ```


Remove/Delete services:

```bash
helmfile destroy
```

Where to host the Helm Charts?
You have two options:

1. Monorepo: Include Helm charts with your application code in a single Git repository.
2. Separate Repository (Preferred): Maintain Helm charts in a dedicated Git repository for better modularity and version control.

````
