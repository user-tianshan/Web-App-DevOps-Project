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


### Milestone 7: Kubernetes Deployment to AKS 


### Milestone 8: CI/CD Pipleline with Azure DevOps


### Milestone 9: AKS Cluster Monitoring


### Milestone 10: AKS Integration with Azure Key Vault for Secrets Management





## License

This project is licensed under the MIT License. For more details, refer to the [LICENSE](LICENSE) file.
