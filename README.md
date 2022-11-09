# K8s Complete Application Deployment
In this example we are deploying an Mongo express app and its database. So K8s componets that we will be using are :

<img src="readmeImages/k8sComponentsUsed.png" data-canonical-src="" width="100" height="100" />

- Deployment/Pods (of course )
- Service (for permanent IP address of pods)
    - Internal (for DB )
    - External (for app)
- Secret (for storing login credentials)
- ConfigMap (for application configration)