# Web-App-DevOps-Project

Welcome to the Web App DevOps Project repo! This application allows you to efficiently manage and track orders for a potential business. It provides an intuitive user interface for viewing existing orders and adding new ones.

## Table of Contents

- [Features](#features)
- [Getting Started](#getting-started)
- [Technology Stack](#technology-stack)
- [Contributors](#contributors)
- [License](#license)

## Features

- **Order List:** View a comprehensive list of orders including details like date UUID, user ID, card number, store code, product code, product quantity, order date, and shipping date.
  
![Screenshot 2023-08-31 at 15 48 48](https://github.com/maya-a-iuga/Web-App-DevOps-Project/assets/104773240/3a3bae88-9224-4755-bf62-567beb7bf692)

- **Pagination:** Easily navigate through multiple pages of orders using the built-in pagination feature.
  
![Screenshot 2023-08-31 at 15 49 08](https://github.com/maya-a-iuga/Web-App-DevOps-Project/assets/104773240/d92a045d-b568-4695-b2b9-986874b4ed5a)

- **Add New Order:** Fill out a user-friendly form to add new orders to the system with necessary information.
  
![Screenshot 2023-08-31 at 15 49 26](https://github.com/maya-a-iuga/Web-App-DevOps-Project/assets/104773240/83236d79-6212-4fc3-afa3-3cee88354b1a)

- **Data Validation:** Ensure data accuracy and completeness with required fields, date restrictions, and card number validation.

## Getting Started

### Prerequisites

For the application to succesfully run, you need to install the following packages:

- flask (version 2.2.2)
- pyodbc (version 4.0.39)
- SQLAlchemy (version 2.0.21)
- werkzeug (version 2.2.3)

### Usage

To run the application, you simply need to run the `app.py` script in this repository. Once the application starts you should be able to access it locally at `http://127.0.0.1:5000`. Here you will be meet with the following two pages:

1. **Order List Page:** Navigate to the "Order List" page to view all existing orders. Use the pagination controls to navigate between pages.

2. **Add New Order Page:** Click on the "Add New Order" tab to access the order form. Complete all required fields and ensure that your entries meet the specified criteria.

## Technology Stack

- **Backend:** Flask is used to build the backend of the application, handling routing, data processing, and interactions with the database.

- **Frontend:** The user interface is designed using HTML, CSS, and JavaScript to ensure a smooth and intuitive user experience.

- **Database:** The application employs an Azure SQL Database as its database system to store order-related data.

## Contributors 

- [Maya Iuga]([https://github.com/yourusername](https://github.com/maya-a-iuga))
- Christopher A. Kennedy

### Milestone 1: Set up the Environment


### Milestone 2: Version Control

- using GitHub
- the project repository was forked and cloned locally.
- feature/add-delivery-date branch was created from the main to isolate the code changes to add a delivery_date column to the orderlist output.
- app.py and order.html were updated to include the delivery date.
- the changes were pushed to the remote repository and  a pull request submitted.
- acting as the PR reviewer, the Pull Request was merged.
- since there was no delivery_date column formatted on the database, a revert-delivery-date branch was created and merged to revert the changes. 

### Milestone 3: Containerisation with Docker

- Dockerfile to create the image using docker build -t mydevops-project .:

  - FROM python:3.8-slim-buster   # Use an official Python runtime as a parent image. You can use `python:3.8-slim`.
  - WORKDIR /app   # Set the working directory in the container
  - COPY . .   # Copy the application files in the container
  - RUN apt-get update && apt-get install -y \   # Install system dependencies and ODBC driver
  - unixodbc unixodbc-dev odbcinst odbcinst1debian2 libpq-dev gcc && \
  - apt-get install -y gnupg && \
  - apt-get install -y wget && \
  - wget -qO- https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
  - wget -qO- https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
  - apt-get update && \
  - ACCEPT_EULA=Y apt-get install -y msodbcsql18 && \
  - apt-get purge -y --auto-remove wget && \  
  - apt-get clean
  - RUN pip install --upgrade pip setuptools   # Install pip and setuptools
  - RUN pip install --trusted-host pypi.python.org -r requirements.txt  # Install Python packages specified in requirements.txt
  - EXPOSE 5000   # Expose port 
  - CMD ["python", "app.py"]  #  Define Startup Command

- The image was tested locally using docker run -p 5000:5000 mydevops-project and url http://127.0.0.1:5000
- The image was then tagged and pushed to dockerhub as 2314sdjafas7/mydevops-project using sudo docker push mydevops-project:latest


### Milestone 4: Defining Networking Services with IaC
- Terraform folder and project name: aks-terraform
- The aks-terraform folder contains two modules: networking-module and aks-cluster-module
- Networking resources and NSG rules:
  - azurerm_resource_group
  - azurerm_virtual_network
  - azurerm_subnet" "control-plane-subnet" address_prefixes = ["10.0.1.0/24"]
  - azurerm_subnet" "worker-node-subnet" address_prefixes = ["10.0.2.0/24"]
  - azurerm_network_security_group" 
  - security_rule , name = "allow-ssh"
  - security_rule , name = "kubi-apiserver-rule"
- Networking module outputs:
   - vnet_id
   - control_plane_subnet_id
   - worker_node_subnet_id
   - networking_resource_group_name
   - aks_nsg_id" {
- Networking module variables:
   - resource_group_name 
   - location
   - vnet_address_space  default = ["10.0.0.0/16"]
- The module was tested and initialised using terraform init


### Milestone 5: Defining an AKS Cluster with IaC
- AKS cluster module input variables:
   - aks_cluster_name
   - cluster_location 
   - dns_prefix 
   - kubernetes_version
   - service_principal_client_id
   - service_principal_secret 
   - vnet_id 
   - control_plane_subnet_id 
   - worker_node_subnet_id 
   - networking_resource_group_name
- AKS cluster module outputs:
  - aks_cluster_name
  - aks_cluster_id
  - aks_kubeconfig
- The module was tested and initialised using terraform init


### Milestone 6: Creating an AKS Cluster with IaC
- Defining the project main.tf configuration
   - provider "azurerm" 
   - module "networking" 
      - source="./networking-module"
   - module "aks_cluster" 
      - source = "./aks-cluster-module"
- Integrating the network module
   - module "networking" {
     - source="./networking-module"
     - resource_group_name = "aks-rg"
     - location = "UK South"
     - vnet_address_space = ["10.0.0.0/16"]
- Integrating the cluster module
   - module "aks_cluster" 
     - source = "./aks-cluster-module"
     - aks_cluster_name = "terraform-aks-cluster"
     - cluster_location = "UK South"
     - dns_prefix = "myaks-project"
     - kubernetes_version = "1.26.6"
     - service_principal_client_id = var.client_id
     - service_principal_secret = var.client_secret
- Apply the main project configuration
   - the project was tested using terraform init, terraform plan, and terrafrom apply.
   - the aks resource group and vnet and subnets were checked and running on Azure
- Accessing the cluster
   - the AKS cluster was accessed and checked locally from the command prompt using:
     - az aks get-credentials --resource-group aks-rg --name terraform-aks-cluster
     - kubectl get nodes
        - NAME                              STATUS   ROLES   AGE   VERSION
        - aks-default-84750897-vmss000000   Ready    agent   16m   v1.26.6
     -  kubectl get services
        - NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
        - kubernetes   ClusterIP   10.0.0.1     <none>        443/TCP   18m



### Milestone 7: Kubernetes Deployment to AKS 
- Kubernetes Manifest Definition - Deployment
   - apiVersion: apps/v1
   - kind: Deployment
   - metadata:
     - name: flask-app-deployment
   - spec:
     - replicas: 2
     - selector:
        - matchLabels:
          - app: flask-app
     - template:
       - metadata:
         - labels:
            - app: flask-app
       - spec:
          - containers:
            - name: mydevops-project
            - image: 2314sdjafas7/mydevops-project  # application image on docker hub
            - ports:
               - containerPort: 5000
      - strategy:
        - type: RollingUpdate # maintains application availability
        - rollingUpdate:
           - maxUnavailable: 1
           - maxSurge: 1

- Kubernets Manifest Defintion - Service
  - apiVersion: v1
  - kind: Service
  - metadata:
    - name: flask-app-service
  - spec:
    - selector:
       - app: flask-app
    - ports:
      - protocol: TCP
      - port: 80 # Port for internal communication within the cluster
      - targetPort: 5000 # Port exposed by container
    - type: ClusterIP # internal service within the AKS cluster

- Deploying Kubernetes Manifest to AKS
   - Following terraform plan, with the AKS cluster running on Azure, a local context was enabled using: 
     - az aks get-credentials --resource-group aks-rg --name terraform-aks-cluster
     - kubectl get nodes
       - NAME                              STATUS   ROLES   AGE     VERSION
       - aks-default-38133135-vmss000000   Ready    agent   4m11s   v1.26.6
     - kubectl get services
       - NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
       - kubernetes   ClusterIP   10.0.0.1     <none>        443/TCP   5m13s
   - manifest applied and deplyed using:
      - kubectl apply -f application-manifest.yaml
         - deployment.apps/flask-app-deployment created
         - service/flask-app-service created

- Testing and Validating Deployments on AKS
  - Pods checked using:
     -  kubectl get pods
        - NAME                                    READY   STATUS    RESTARTS   AGE
        - flask-app-deployment-7b69759748-qlrkw   1/1     Running   0          79s
        - flask-app-deployment-7b69759748-vjdbj   1/1     Running   0          79s
  - port forwarding initiated using
     - kubectl port-forward flask-app-deployment-7b69759748-qlrkw 80:5000
  - cluster accessed locally and application functionality checked at http://127.0.0.1
       

### Milestone 8: CI/CD Pipleline with Azure DevOps
- Create new DevOps project
  - The DevOps project was setup within Azure DevOps as the foundation for the PipeLine. The project name was Azure DevOps Project
- Pipeline Setup
   - The source repository was configured as GitHub using the existing project repository on GitHub
   - A Starter Pipeline template was created from Azure DevOps
- Docker Hub Service Connection
   - A personal access token was generated on Docker Hub
   - A docker service connection was then built and tested from the project settings service connections
- Docker Image Build and Push
   - The Docker Build and Push command was selected from the Docker tasks lists and added to the pipeline
     - task: Docker@2
        - inputs:
          - containerRegistry: 'Docker'
          - repository: '2314sdjafas7/azuredevopsproject'
          - command: 'buildAndPush'
          - Dockerfile: '**/Dockerfile'
          - tags: 'latest'
   - trigger: - main ensures that the pipeline automatically runs each time there is a push to the main
- AKS Service Connection
   - The AKS service connection was built using the project settings, service connection, New service connection, Kubernetes, option
   - This method requires an existing AKS cluster.
- Deployment to AKS Cluster
   - the Deploy to Kubernetes option from the task list was used to generate the deployment to AKS cluster using the AKS service connection and deploy action from the list, and added to the pipeline yaml file.
        - task: KubernetesManifest@1
           - inputs:
              - action: 'deploy'
              - connectionType: 'kubernetesServiceConnection'
              - kubernetesServiceConnection: 'AKS service connection'
              - manifests: 'application-manifest.yaml'
- Testing and Validation
  - The pipeline was run and tested locally from the command prompt.
    - az aks get-credentials --resource-group VisualStudioOnline-C10BF13F577D425BB6E0029965A2011B --name DevOps-AKS-Cluster
       - Merged "DevOps-AKS-Cluster" as current context in C:\Users\tians\.kube\config
    - kubectl get nodes
      - NAME                                STATUS   ROLES   AGE   VERSION
      - aks-agentpool-22468329-vmss000000   Ready    agent   17h   v1.27.7
      - aks-agentpool-22468329-vmss000001   Ready    agent   17h   v1.27.7
      - aks-userpool-22468329-vmss000000    Ready    agent   17h   v1.27.7
      - aks-userpool-22468329-vmss000001    Ready    agent   17h   v1.27.7
    - kubectl get services
      - NAME                TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
      - flask-app-service   ClusterIP   10.0.66.32   <none>        80/TCP    33m
      - kubernetes          ClusterIP   10.0.0.1     <none>        443/TCP   17h
    - kubectl get pods
      -  NAME                                    READY   STATUS    RESTARTS   AGE
      - flask-app-deployment-57557b989d-rw2zl   1/1     Running   0          15m
      - flask-app-deployment-57557b989d-v5bvf   1/1     Running   0          15m
  - port forwarding initiated using:  
    - kubectl port-forward flask-app-deployment-57557b989d-rw2zl 80:5000
  - cluster accessed locally and application functionality checked at http://127.0.0.1


### Milestone 9: AKS Cluster Monitoring
- Container Insights
   - Enabled for collecting real-time in-depth performance and diagnostic data.
- Metrics Explorer Charts
    - Average Node CPU Usage:
    - Average Pod Count:
    - Used Disk Percentage:
     - ![Screenshot](https://github.com/user-tianshan/aks-terraform/blob/main/chart1.jpg)
    - Bytes Read and Written per Second:
     - ![Screenshot](https://github.com/user-tianshan/aks-terraform/blob/main/chart2.jpg)
- Log Analytics
   - Average Node CPU Usage Percentage per Minute:
   - Average Node Memory Usage Percentage per Minute:
   - Pods Counts with Phase:
   - Find Warning Value in Container Logs:
      - let FindString = "warning";//Please update term you would like to find in LogEntry here ContainerLog | where LogEntry has FindString |take 100
   - Monitoring Kubernetes Events:
      - // Kubernetes events // Lists all the Kubernetes events. KubeEvents | where TimeGenerated > ago(7d) | where not(isempty(Namespace)) | top 200 by TimeGenerated desc
- Alert Rules and Alarms
  - Alert rule to trigger an alarm when the used disk percentage in the AKS cluster exceeds 90%
     - Check every 5 minutes with loopback period of 15 minutes.
     - Notifications sent to email address.
     - Alert rules for CPU usage and memory working set percentage to trigger when they exceed 80%.


### Milestone 10: AKS Integration with Azure Key Vault for Secrets Management
- Azure Key Vault
   - Key vault created on azure with Role Based Access Control
     - subscription: Christopher Kennedy DevOps
     - resource group: aks-rg
     - key vault name: aks-rg-key-vault
   - Vault URI: https://aks-rg-key-vault.vault.azure.net/
- Assigning Key Vault Administrator Role
   - Key Vault Administrator role added from the Access Control(IAM) tab of the key vault home page 
- Create Key Vault Secrets
  - Secrets added to the key vault from the Ojects secrets tab:
    - server-name
    - database-name
    - server-username
    - server-password
- Managed Identities for AKS
- Managed Identiy Permissions
- Modify code for managed identity credentials
  - managed identity code added to app.py file:
    - import azure.identity
    - import azure.keyvault.secrets
    - from azure.identity import ManagedIdentityCredential
    - from azure.keyvault.secrets import SecretClient
    - key_vault_url = "https://aks-rg-key-vault3.vault.azure.net/"
    - credential = ManagedIdentityCredential(client_id="6951469a-c389-4319-b25f-1022926fa1ee")
    - secret_client = SecretClient(vault_url=key_vault_url, credential=credential)
    - server_name = secret_client.get_secret("server-name")
    - database_name = secret_client.get_secret("database-name")
    - server_username = secret_client.get_secret("server-username")
    - server_password = secret_client.get_secret("server-password")
    - server = server_name.value
    - database = database_name.value
    - username = server_username.value
    - password = server_password.value
  - libraries added to Dockerfile requirements.txt file:
    - azure-identity==1.15.0
    - azure-keyvault-secrets==4.7.0
- End-to-End Pipeline testing





## License

This project is licensed under the MIT License. For more details, refer to the [LICENSE](LICENSE) file.
