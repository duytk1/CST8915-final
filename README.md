Updated Application Architecture:
![Diagram](/images/diagram.png)  

Application and Architecture Explanation:
The architecture of this project will work as follows:
The user can go to the store front page and place an order
The store-front written in VueJS will render a user interface for them to view the products and add to cart then place the order
The order will then be sent to order-service in NodeJS which is handled by the order-queue by Azure Service Bus
The product-service will also handle the order in rust
The employee of the store can go to the store-admin page which can handle the order in makeline-service written in golang
For any order that is processed it will be removed from the queue which Azure Service Bus will take care of and remove from the database
There is also the AI Services that Azure provide with Azure OpenAI that gives us access to GPT4 for description and DALL-E for image generation which will be used on the product page

Deployment Instructions:

Step 0: install prerequisites

Install kubectl: https://kubernetes.io/docs/tasks/tools/
Install Python: https://www.python.org/downloads/
Install node: https://nodejs.org/en/download/package-manager
Install Azure CLI: https://learn.microsoft.com/en-us/cli/azure/install-azure-cli

Step 1: Create Kubernetes Cluster on Azure:

Search and go to Kubernetes Services on Azure
Click Create -> Kubernetes Cluster

![1.0](/images/1.0.png)

![1.1](/images/1.1.png)

Choose the following settings:
Region: Canada Central
Availability zone: none
AKS Pricing Tier: free
Automatic upgrade: disabled
Node security type channel: none

![1.2](/images/1.2.png)
Click the Add Node Pool button


![1.3](/images/1.3.png)Choose the following settings:
Node pool name: workerspool
OS SKU: Ubuntu Linux
Node Size: D2as_v4
Node count: 1

Click on the already existing node to update

Node pool name: masterpool
OS SKU: Ubuntu Linux
Node Size: D2as_v4
Node count: 1

Click Review + Create to create the AKS cluster

![1.4](/images/1.4.png)

Step 2: Deploy the services

Connect to AKS:
Run command

az login

Choose your subscription

Set your subscription to correct subscription with command:

az account set --subscription 'subscribtion-id'

az aks get-credentials --resource-group AlgonquinPetStoreRG --name AlgonquinPetStoreCluster

Replace the subscription-id and AlgonquinPetStoreRG and AlgonquinPetStoreCluster with your information

To deploy the algonquin petstore run the command:

kubectl apply -f deploymentfiles/algonquin-pet-store-all-in-one.yaml

Verify deployment with commands:

kubectl get pods
kubectl get services

Test your store front and product service:

Go to your AKS Cluster -> Services and Ingresses -> Go to the external links on store front and store admin

![2.1](/images/2.1.png)
Deploy all other deployment files in the folder DeploymentFiles with commands:

kubectl apply -f deploymentfiles/admin-tasks.yaml
kubectl apply -f deploymentfiles/aps-all-in-one.yaml
kubectl apply -f deploymentfiles/config-maps.yaml
kubectl apply -f deploymentfiles/my-deployment.yaml
kubectl apply -f deploymentfiles/my-service.yaml



Table of Microservice Repositories:
| Service Name      | Link                                                                 |
|-------------------|----------------------------------------------------------------------|
| **Store-Front**    | [duytk1/store-front](https://hub.docker.com/repository/docker/duytk1/store-front/general) |
| **Store-Admin**    | [duytk1/store-admin](https://hub.docker.com/repository/docker/duytk1/store-admin/general) |
| **Order-Service**  | [duytk1/order-service](https://hub.docker.com/repository/docker/duytk1/order-service/general) |
| **Product-Service**| [duytk1/product-service](https://hub.docker.com/repository/docker/duytk1/product-service/general) |
| **Makeline-Service**| [duytk1/makeline-service](https://hub.docker.com/repository/docker/duytk1/makeline-service/general) |
| **AI-Service**     | [duytk1/ai-service](https://hub.docker.com/repository/docker/duytk1/ai-service/general) |
| **Database**       | [MongoDB GitHub](https://github.com/duytk1/CST8915-final/tree/master/mongo) |


Table of Docker Images:
| Service Name      | Link                                                                 |
|-------------------|----------------------------------------------------------------------|
| **Store-Front**    | [Store-Front GitHub](https://github.com/duytk1/CST8915-final/tree/master/store-front) |
| **Store-Admin**    | [Store-Admin GitHub](https://github.com/duytk1/CST8915-final/tree/master/store-admin) |
| **Order-Service**  | [Order-Service GitHub](https://github.com/duytk1/CST8915-final/tree/master/order-service) |
| **Product-Service**| [Product-Service GitHub](https://github.com/duytk1/CST8915-final/tree/master/product-service) |
| **Makeline-Service**| [Makeline-Service GitHub](https://github.com/duytk1/CST8915-final/tree/master/makeline-service) |
| **AI-Service**     | [AI-Service GitHub](https://github.com/duytk1/CST8915-final/tree/master/ai-service) |


Any issues or limitations in the implementation (Optional)

Demo Video
https://youtu.be/icwRRbdkX3w