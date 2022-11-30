### Node app with the mongoDB with minimum three replicas
![image](https://user-images.githubusercontent.com/97250268/204542447-5d278aff-218a-4325-90f1-6a2e751fc947.png)

### Kubernetes Cluster node:
![image](https://user-images.githubusercontent.com/97250268/204653329-e7c63a79-11a7-41a3-a1c8-9eb31bb00055.png)

### What is deployment?
- A Kubernetes Deployment is used to tell Kubernetes how to create or modify instances of the pods that hold a containerized application. Deployments can scale the number of replica pods, enable rollout of updated code in a controlled manner, or roll back to an earlier deployment version if necessary.
### What is POD?
- Pod is a smallest deployable unit of computing that you can create and manage in Kubernetes.
### Steps for the deployment of node app
- create node-deployment.yml and node-service.yml to deploy node app
- code for node-deployement.yml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node
spec:
  selector:
    matchLabels:
      app: node
  replicas: 3
  template:
    metadata:
      labels:
        app: node
    spec:
      containers:
        - name: node
          image: meghanasrividya/eng130-nodeapp
          ports:
            - containerPort: 3000
          env:
            - name: DB_HOST
              value: mongodb://mongo:27017/posts
          #command: [""]
          args: ["node", "seeds/seed.js"]
          imagePullPolicy: Always

```
- Code for node-service.yml
```
apiVersion: v1
kind: Service
metadata:
  name: node
spec:
  selector:
    app: node
  ports:
    - port: 3000
      targetPort: 3000
  type: LoadBalancer

```
  - Use the below command to deploy
   `kubectl create -f node-deployment.yml`
   `kubectl create -f node-service.yml`
- cretae mongo-deployment.yml and mongo-service.yml to setup database.
- Code for mongo-deployement.yml
```
#mongo-deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo
  labels:
    app.kubernetes.io/name: mongo
    app.kubernetes.io/component: backend
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: mongo
      app.kubernetes.io/component: backend
  replicas: 1
  template:
    metadata:
      labels:
        app.kubernetes.io/name: mongo
        app.kubernetes.io/component: backend
    spec:
      containers:
      - name: mongo
        image: mongo:4.0.4
        args:
          - --bind_ip
          - 0.0.0.0
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 27017

```
- Code for mongo-service.yml
```
# mongo-service.yml

apiVersion: v1
kind: Service
metadata:
  name: mongo
  labels:
    app.kubernetes.io/name: mongo
    app.kubernetes.io/component: backend
spec:
  ports:
  - port: 27017
    targetPort: 27017
  selector:
    app.kubernetes.io/name: mongo
    app.kubernetes.io/component: backend

```
  - Use the below commands to deploy
    `kubectl create -f mongo-deployment.yml`
    `kubectl create -f mongo-service.yml`
    
![image](https://user-images.githubusercontent.com/97250268/204874976-34b99213-e4ea-4201-9f14-866a90aa2358.png)

![image](https://user-images.githubusercontent.com/97250268/204875100-66f93a7e-e209-4183-88f0-8f0d97d78781.png)

- Go to the browser and you can see the app and posts are running at localhost

![image](https://user-images.githubusercontent.com/97250268/204875422-bccc646e-6112-43f1-92d2-6db3529a2ad8.png)


![image](https://user-images.githubusercontent.com/97250268/204875665-41ef497c-b72b-4375-bc9e-79f642163b11.png)

