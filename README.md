# Deploy a Microservices Application to a Kubernetes Cluster

## Description

This project demonstrates how to deploy the [Online Boutique demo application](https://github.com/GoogleCloudPlatform/microservices-demo), a cloud-native microservices application, into a Kubernetes cluster hosted on [Linode](https://www.linode.com/de/).

Before you begin, it’s important to understand the following:

- Which microservices are included in the deployment  
- How the services are connected  
- What dependencies each service has (e.g., databases, third-party services)  
- Which microservice is publicly accessible (i.e., the entry point to the cluster)  
- A visual diagram of service interactions is recommended for better understanding  

---

## Setup

### Prerequisites

- A Linode account with access to create a Kubernetes cluster  
- Minikube installed and running on your local machine for local testing  
  - Refer to the [Minikube official documentation](https://minikube.sigs.k8s.io/docs/start/) for installation instructions  
  - Minikube requires either a container runtime (e.g., Docker) or a virtual machine manager (e.g., VirtualBox). See the [Minikube drivers documentation](https://minikube.sigs.k8s.io/docs/drivers/) for details  
  - To start Minikube with Docker as the preferred driver:

    ```bash
    minikube start --driver=docker
    ```

- `kubectl` installed for interacting with both local and cloud-based Kubernetes clusters  
  > Note: `kubectl` is included with Minikube by default, so a separate installation may not be necessary  

---

## Deployment Steps

### 1. Create Deployment and Service YAMLs (Optional)

- A ready-to-use `config.yaml` file is already provided for deployment 
- If needed, you can use the included skeleton file `config_skeleton.yaml` to create custom Deployments and Services for each microservice  
- Make sure to define correct resource, labels, ports, and dependencies

### 2. Launch a Kubernetes Cluster on Linode

- Log in to your Linode account  
- Create a new Kubernetes cluster:
  - Choose a name  
  - Select your nearest region  
  - Add 3 small nodes (shared CPU)  
- Once the cluster is ready and nodes are running, download the **kubeconfig** file (used to authenticate and connect to your cluster)

### 3. Configure Access to the Cluster

- Set the proper permissions on the kubeconfig file:

  ```bash
  chmod 400 <your-kubernetesconfig-file.yaml>

- Export the kubeconfig file as an environment variable:
  ```bash
  export KUBECONFIG=<your-kubernetesconfig-file.yaml>
- Test the connection:
  ```bash
  kubectl get nodes
  
### 4. Deploy Microservices to the Cluster

- Create a namespace:
  ```bash
  kubectl create namespace microservices
  
- Deploy the microservices into the namespace:
  ```bash
  kubectl apply -f config.yaml -n microservices

### 5. Verify the Deployment

- Check that all pods are running:
  ```bash
  kubectl get pods -n microservices
  
- Check available services:
  ```bash
  kubectl get svc -n microservices

### 6. Access the Application in a Browser
- Go to your Linode Kubernetes dashboard
- Choose a public IP from one of the worker nodes
- Use the assigned NodePort (30007) to access the application:
  ```ccp
  http://<worker-node-ip>:30007

### Screenshots
<img width="800" height="464" alt="Screenshot 2025-07-24 at 08 39 52" src="https://github.com/user-attachments/assets/5e2d3888-fd3e-4eed-b8ab-6ad0961687d3" />
<img width="800" height="545" alt="Screenshot 2025-07-24 at 08 54 00" src="https://github.com/user-attachments/assets/ac76f12c-b868-4878-9bb5-d9ac2e59ddc3" />


