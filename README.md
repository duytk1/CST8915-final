# Updated Application Architecture
![Diagram](/images/diagram.png)  

## Application and Architecture Explanation:
The architecture of this project will work as follows:

1. The user can go to the store front page and place an order.
2. The store-front, written in VueJS, will render a user interface for them to view the products, add to cart, and place the order.
3. The order will then be sent to `order-service` in NodeJS, which is handled by the order-queue via Azure Service Bus.
4. The `product-service` will also handle the order in Rust.
5. The employee of the store can go to the `store-admin` page, which can handle the order in `makeline-service` written in Golang.
6. For any order that is processed, it will be removed from the queue, and Azure Service Bus will take care of removing it from the database.
7. There is also the AI Services that Azure provides through Azure OpenAI, giving us access to GPT-4 for product descriptions and DALL-E for image generation, which will be used on the product page.

---

## Deployment Instructions:

### Step 0: Install Prerequisites

- Install `kubectl`: [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/)
- Install Python: [Python Download](https://www.python.org/downloads/)
- Install Node.js: [Node.js Download](https://nodejs.org/en/download/package-manager)
- Install Azure CLI: [Azure CLI Installation](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)

### Step 1: Create Kubernetes Cluster on Azure

1. Go to **Kubernetes Services** on Azure.
2. Click **Create** -> **Kubernetes Cluster**.

   ![1.0](/images/1.0.png)

   ![1.1](/images/1.1.png)

3. Choose the following settings:
   - Region: **Canada Central**
   - Availability zone: **None**
   - AKS Pricing Tier: **Free**
   - Automatic upgrade: **Disabled**
   - Node security type channel: **None**

   ![1.2](/images/1.2.png)

4. Click the **Add Node Pool** button.

   ![1.3](/images/1.3.png)

   Choose the following settings:
   - Node pool name: **workerspool**
   - OS SKU: **Ubuntu Linux**
   - Node Size: **D2as_v4**
   - Node count: **1**

5. Click on the already existing node to update.

   - Node pool name: **masterpool**
   - OS SKU: **Ubuntu Linux**
   - Node Size: **D2as_v4**
   - Node count: **1**

6. Click **Review + Create** to create the AKS cluster.

   ![1.4](/images/1.4.png)

### Step 2: Deploy the Services

1. **Connect to AKS:**
   - Run command:
     ```bash
     az login
     ```
   - Choose your subscription:
     ```bash
     az account set --subscription 'subscription-id'
     ```
   - Get credentials for the cluster:
     ```bash
     az aks get-credentials --resource-group AlgonquinPetStoreRG --name AlgonquinPetStoreCluster
     ```

   Replace `subscription-id`, `AlgonquinPetStoreRG`, and `AlgonquinPetStoreCluster` with your information.

2. **Deploy the Algonquin PetStore:**
   Run the following command:
   ```bash
   kubectl apply -f deploymentfiles/algonquin-pet-store-all-in-one.yaml
    ```

3. **Verify deployment with commands**:

    kubectl get pods
    kubectl get services
    
4. **Test your storefront and product service**:

    Go to AKS Cluster -> Services and Ingresses.
    Visit the external links for store-front and store-admin.
![2.1](/images/2.1.png)

5. **Deploy all other deployment files in the DeploymentFiles folder**:
Run the following commands:

    kubectl apply -f deploymentfiles/admin-tasks.yaml
    kubectl apply -f deploymentfiles/aps-all-in-one.yaml
    kubectl apply -f deploymentfiles/config-maps.yaml
    kubectl apply -f deploymentfiles/my-deployment.yaml
    kubectl apply -f deploymentfiles/my-service.yaml
    
    

    
Table of Microservice Repositories:
| Service Name      | Link                                                                 |
|-------------------|----------------------------------------------------------------------|
| **Store-Front**    | [Store-Front GitHub](https://github.com/duytk1/CST8915-final/tree/master/store-front) |
| **Store-Admin**    | [Store-Admin GitHub](https://github.com/duytk1/CST8915-final/tree/master/store-admin) |
| **Order-Service**  | [Order-Service GitHub](https://github.com/duytk1/CST8915-final/tree/master/order-service) |
| **Product-Service**| [Product-Service GitHub](https://github.com/duytk1/CST8915-final/tree/master/product-service) |
| **Makeline-Service**| [Makeline-Service GitHub](https://github.com/duytk1/CST8915-final/tree/master/makeline-service) |
| **AI-Service**     | [AI-Service](https://github.com/duytk1/CST8915-final/tree/master/ai-service) |
| **Database**       | [Mongo Database](https://github.com/duytk1/CST8915-final/tree/master/mongo) |


Table of Docker Images:
| Service Name      | Link                                                                 |
|-------------------|----------------------------------------------------------------------|
| **Store-Front**    | [duytk1/store-front](https://hub.docker.com/repository/docker/duytk1/store-front/general) |
| **Store-Admin**    | [duytk1/store-admin](https://hub.docker.com/repository/docker/duytk1/store-admin/general) |
| **Order-Service**  | [duytk1/order-service](https://hub.docker.com/repository/docker/duytk1/order-service/general) |
| **Product-Service**| [duytk1/product-service](https://hub.docker.com/repository/docker/duytk1/product-service/general) |
| **Makeline-Service**| [duytk1/makeline-service](https://hub.docker.com/repository/docker/duytk1/makeline-service/general) |
| **AI-Service**     | [duytk1/ai-service](https://hub.docker.com/repository/docker/duytk1/ai-service/general) |


Any issues or limitations in the implementation (Optional)

Demo Video
https://youtu.be/icwRRbdkX3w