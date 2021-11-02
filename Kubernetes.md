**pod** is a layer of abstraction on top of containers 

**deployment**  is another abstraction on top of pods which makes it more convenient to interact with the pods replicate them and do some other configuration

```sh
kubectl create deployment nginx-dep1 --image=nginx
kubectrl get deployment
kubectrl get pod
kubectrl get replicaset
kubectrl edit deployment
```

```sh
kubectrl logs [pod name]
kubectrl describe pod [pod name]
kubectrl exec -it [pod name] -- bin/bash
```

```sh
kubectrl get deployment
kubectrl get pod
kubectrl delete deployment nginx-dep1
kubectrl apply -f [file name]
```

```
vim nginx-deployment.yaml
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.16
        ports:
        - containerPort: 80
```

- CRUD commands

```
kubectrl apply -f nginx-deployment.yaml
```

| Opt               | Command                          |
| ----------------- | -------------------------------- |
| Create deployment | kubectl create deployment [name] |
| Edit deployment   | kubectl edit deployment [name]   |
| Delete deployment | kubectl delete deployment [name] |

- Status of different k8s components 

```
kubectl get nodes | pod  | services | replicaset | deployment 
```

- Debugging pods

| Opt                      | Command                                                   |
| ------------------------ | --------------------------------------------------------- |
| Log to console           | kubectl logs [pod name]                                   |
| Get Interactive Terminal | kubectl exec -it kubectrl exec -it [pod name] -- bin/bash |

- Use configuration file for CRUD

| Opt                            | Command                       |
| ------------------------------ | ----------------------------- |
| Apply a configuration file     | kubectl apply -f [file name]  |
| Delete with configuration file | kubectl delete -f [file name] |

```
kubectl get pod -o wide
kubectl get deployment nginx-deployment -o yaml > nginx-deployment-result.yaml 
```



