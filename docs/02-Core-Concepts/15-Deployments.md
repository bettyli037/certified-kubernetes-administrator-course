# Deployments
  - Take me to [Video Tutorial](https://kodekloud.com/topic/deployments-3/)

In this section, we will take a look at kubernetes deployments

#### Deployment is a kubernetes object. 
  
 ![deployment](../../images/deployment.PNG)

 Deployment resource has the following features:
  1. Update pods to new versions
  Rollover
  2. Rolling back a deployment
  3. Scaling a Deployment
  4. Pausing and Resuming a rollout of a Deployment

#### How do we create deployment?

```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: myapp-deployment
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3
     selector:
       matchLabels:
        type: front-end
 ```
- Once the file is ready, create the deployment using deployment definition file
  ```
  $ kubectl create -f deployment-definition.yaml
  ```
- To see the created deployment
  ```
  $ kubectl get deployment
  ```
- The deployment automatically creates a **`ReplicaSet`**. To see the replicasets
  ```
  $ kubectl get replicaset
  ```
- The replicasets ultimately creates **`PODs`**. To see the PODs
  ```
  $ kubectl get pods
  ```
    
  ![deployment1](../../images/deployment1.PNG)
  
- To see the all objects at once
  ```
  $ kubectl get all
  ```
  ![deployment2](../../images/deployment2.PNG)
  
K8s Reference Docs:
- https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/
- https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/
- https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/


### Personal Notes:
1. You must specify an appropriate selector and Pod template labels in a Deployment (in this case, app: nginx).

Do not overlap labels or selectors with other controllers (including other Deployments and StatefulSets). Kubernetes doesn't stop you from overlapping, and if multiple controllers have overlapping selectors those controllers might conflict and behave unexpectedly.

2. Rollout: A Deployment's rollout is triggered if and only if the Deployment's Pod template (that is, .spec.template) is changed, for example if the labels or container images of the template are updated. Other updates, such as scaling the Deployment, do not trigger a rollout.

Dpeloyment vs. ReplicaSet

A ReplicaSet ensures that a specified number of pod replicas are running at any given time. However, Dpeloyment is a higher-level concept that manages ReplicaSets and provides declarative updates to Pods along with a lot of other useful features.

Deployment resource makes it easier for updaing your pods to a newer version.
Lets say you use ReplicaSet-A for controlling your pods, then you wish to update your pods to a newer version, now you should do the following rolling update:
  - Create Replicaset-B
  - Scale down ReplicaSet-A
  - Scale up ReplicaSet-B

Although this does the job, but it's not a good practice and it's better to let K8s do the job.
A deployment resource does this automatically without any human interaction and increases the abstraction by one level.

### Certification Tips
1. Using the `kubectl run` command to help in generating a YAML template. (Since it might be a bit difficult to create and edit YAML files.)

Use the following commands
1. Create an Nginx Pod
`kubectl run nginx --image=nginx`
2. Generate POD Manifest YAML file (-o yaml). Don't create the pod (-dry-run).
`kubectl run nginx --image=nginx --dry-run=client -o yaml`
3. Create a deployment
`kubectl create deployment --image=nginx nginx`
4. Generate Deployment yaml file (-o yaml). Don't create it (-dry-run) with 4 Replicas (-replicas=4)
`kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml`
5. We can specify the -replicas option to create a deployment with 4 replicas.
`kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml`