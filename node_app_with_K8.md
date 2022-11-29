### Node app with the mongoDB with minimum three replicas
![image](https://user-images.githubusercontent.com/97250268/204542447-5d278aff-218a-4325-90f1-6a2e751fc947.png)
### What is deployment?
- A Kubernetes Deployment is used to tell Kubernetes how to create or modify instances of the pods that hold a containerized application. Deployments can scale the number of replica pods, enable rollout of updated code in a controlled manner, or roll back to an earlier deployment version if necessary.
### What is POD?
- Pod is a smallest deployable unit of computing that you can create and manage in Kubernetes.
### Steps for the deployment of node app
- create node-deployment.yml and node-service.yml to deploy node app
- cretae mongo-deployment.yml and mongo-service.yml to setup database.
