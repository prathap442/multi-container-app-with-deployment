# Section14
 - the image that is shown in the 'shot1.png' would form as the basis for the sake of the building the entire workflow for this section
- We can even create one single file where the clusteripservice and the deployment object also exists in the same file .
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: server-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      component: server
  template:
    metadata:
      labels:
        component: server
    spec:
      containers:
        - name: server
          image: stephengrider/multi-server
          ports:
            - containerPort: 5000        
---

apiVersion: v1
kind: Service
metadata:
  name: server-cluster-ip-service
spec:
  type: clusterIP
  selector:
    component: server
  ports:
    - port: 5000
      targetPort: 5000

```

As shown in the above code if we create a file by name server-config.yaml and place the above code there and then do 

```
$ kubectl apply -f server-config.yaml
```

both the deployment object configuration and then clusterip configuration would be done because of the above command .



Now we will work on the redis config files
and we will create the following things

redis-cluster-ip-service.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: client-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: web
  ports:
    - port: 3000
      targetPort: 3000
```


redis-deployment-service.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: redis
  template:
    metadata:
      labels:
        component: redis
    spec:
      containers:
        - name: redis
          image: redis
          ports:
            - containerPort: 6379
```