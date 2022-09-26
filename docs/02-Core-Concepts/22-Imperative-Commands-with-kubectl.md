# Certification Tips - Imperative Commands with kubectl
  - Take me to the [Certification tips page](https://kodekloud.com/topic/certification-tips-imperative-commands-with-kubectl/)

## Imperative
It is hard to work in complex scenario.
There is no records of these steps.

### Create objects
#### Create a Pod
```
kubectl run --image=nginx nginx
```

Create a deployment
```
kubectl create deploymnet --image=nginx
```

#### Create a Service
``` kubectl expose deployment nginx --port 80

###Update objects

```
kubectl edit deployment nginx
```
The pod-definition file will be created and saved in kubernetes memory. It is different from the local definition. After you apply the changes, this file will disappear.

```
kubectl replace -f nginx.yaml
```
In this way, the changes are recorded and can be tracked.

```
kubectl replace --force -f nginx.yaml
```
In this way, you completely delete the object and create a new one.

```
kubectl scale deployment nginx --replicas=5
```
```
kubectl set image deployment nginx nginx=nginx:1.18
```

--dry-run: By default as soon as the command is run, the resource will be created. If you simply want to test your command , use the --dry-run=client option. This will not create the resource, instead, tell you whether the resource can be created and if your command is right.

-o yaml: This will output the resource definition in YAML format on screen.

Use the above two in combination to generate a resource definition file quickly, that you can then modify and create resources as required, instead of creating the files from scratch.

Tips:
Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379

```
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
```

(This will automatically use the pod’s labels as selectors)

Or
```
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml 
```
(This will not use the pods labels as selectors, instead it will assume selectors as app=redis. You cannot pass in selectors as an option. So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service)

Create a Service named nginx of type NodePort to expose pod nginx’s port 80 on port 30080 on the nodes:

```
kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml
```
(This will automatically use the pod’s labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

Or

```
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```
(This will not use the pods labels as selectors)

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the `kubectl expose` command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.

