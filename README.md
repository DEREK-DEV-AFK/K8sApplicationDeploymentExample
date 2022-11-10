# K8s Complete Application Deployment
In this example we are deploying an Mongo express app and its database. So K8s componets that we will be using are :

<img src="readmeImages/k8sComponentsUsed.png" data-canonical-src="" width="100" height="100" />

- Deployment/Pods (of course )
- Service (for permanent IP address of pods)
    - Internal (for DB )
    - External (for app)
- Secret (for storing login credentials)
- ConfigMap (for application configration)
## Browser Request Flow through the K8s Components
<img src="readmeImages/workflow.png" width="600" />

## Creating YAML files
### Step 1 : Mongo DB Deployment (database)
note: k8s cluster should be running
- Creating `Deployment` file for MongoDB `fileName:mongodbDeploy.yaml`
    - requirements :
        - ENV variables :
            - `MONGO_INITDB_ROOT_USERNAME`
            - `MONGO_INITDB_ROOT_PASSWORD` 
- Create `Secret` file for storing login credentials `fileName:mongodbSecret.yaml`
    - requirements :
        - Username & Password in base64 encoded
- NOTE: You need to deploy secret before Deployment file, because we are referencing it in deployment
- Deploying `Secret` file
    - To deploy the CLI commands is
        ```
        kubectl apply -f mongodbSecret.yaml
        ```
    - To see the list of Secret in your machine
        ```
        kubectl get secret
        ```
- Now we can refrence it in our application deployment file    
- Deploying `mongodbDeploy.yaml` file
    - To deploy the CLI commands is
        ```
        kubectl apply -f mongodbDeploy.yaml
        ```
    - To see the list of Pod in your machine
        ```
        kubectl get pod
        ```
    - To see the describe of sepecific pod
        ```
        kubectl describe pod <nameOfPod>
        ```
- Creating service configuration
    - This config will be inside same file `fileName:mogodbDeploy.yaml`
    - For better practise this is the way
- Deploy service 
    - To deploy, the CLI command is 
    ```
    kubectl apply -f mongodbDeploy.yaml
    ```    
    - To see the list of Service in your machine
    ```
    kubectl get service
    ```
    - To see the description of specific service
    ```
    kubectl describe service <nameOfService>
    ```
    - To get all components which start with "mongo"
    ```
    kubectl get all | grep mongo
    ```
### Step 2 : Mongo express deployment
- Creating `Deployment` file for mongo express `fileName:mongoExpress.yaml`
    - Requirement of mongo express
        - ENV variables :
            1. `ME_CONFIG_MONGODB_SERVER` : (which database to connect, it will be mongDB Address / internal service)
            2. `ME_CONFIG_MONGODB_ADMINUSERNAME` : (username for admin credentials)
            3. `ME_CONFIG_MONGODB_ADMINPASSWORD` : (password for admin credentials)
- Creating `configMap` file for database url `fileName:mongoConfigMap.yaml`
    - Takes service name as url
- Deploying `configMap` file first in order to use it in `Deployment` of mongoExpress
    - To deploy, CLI command is
    ```
    kubectl apply -f mongoConfigMap.yaml
    ```
    - To get list of configmap
    ```
    kubectl get configmap
    ```
    - To get description of specific confimap
    ```
    kubectl describe configmap <nameOfConfimap>
    ```
- Deploying `mongoExpress.yaml` file
    - To deploy, CLI command is
    ```
    kubectl apply -f mongoExpress.yaml
    ```
    - To get list of pods deployed
    ```
    kubectl get pod
    ```
    - To get detail of specific pod
    ```
    kubectl describe pod <nameOfPod>
    ```
    - To get logs of specific pod
    ```
    kubectl logs <nameofPod>
    ```
- Creating an `External Service` for mongoExpress pod `fileName:mongoExpress.yaml`
    - To deploy, CLi command is
    ```
    kubectl apply -f mongoExpress.yaml
    ```
    - To get the list of service
    ```
    kubectl get service
    ```