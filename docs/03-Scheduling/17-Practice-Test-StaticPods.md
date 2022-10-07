# Practice Test - Static Pods
  - Take me to [Practice Test](https://kodekloud.com/topic/practice-test-static-pods/)
  
Solutions to the practice test - static pods
- Run the command kubectl get pods --all-namespaces and look for those with -master appended in the name
  
  <details>

  ```
  $ kubectl get pods --all-namespaces
  $ kubectl get pods -A
  ```
  </details>

Notes:
When we check the ownerReferences of the static pods by looking at the yaml file with -o yaml in the command, we will notice that the kind is Node. That is one way to be sure whether a pod is static pod or not. For other normal pods, the kind of the owner might be ReplicaSet.

We can also use more advanced command like selector and filter to find it out whether the kind of the owner is node or not.

- Which of the below components is NOT deployed as a static pod?

  <details>

  ```
  $ kubectl get pods --all-namespaces
  ```
  </details>

- Which of the below components is NOT deployed as a static POD?

  <details>

  ```
  $ kubectl get pods --all-namespaces
  ```
  </details>

- Run the kubectl get pods --all-namespaces -o wide

  <details>

  ```
  $ kubectl get pods --all-namespaces -o wide
  ```
  </details>

- Run the command ps -aux | grep kubelet and identify the config file - --config=/var/lib/kubelet/config.yaml. Then checkin the config file for staticPdPath.

  <details>

  ```
  $ ps -aux | grep kubelet
  ```
  </details>

- Count the number of files under /etc/kubernetes/manifests

- Check the image defined in the /etc/kubernetes/manifests/kube-apiserver.yaml manifest file.

- Create a pod definition file in the manifests folder. Use command kubectl run --restart=Never --image=busybox static-busybox --dry-run -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml
  
  <details>

  ```
  $ kubectl run --restart=Never --image=busybox static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml
  ```
  </details>

- Simply edit the static pod definition file and save it 

  <details>

  ```
  /etc/kubernetes/manifests/static-busybox.yaml
  ```
  </details>

  OR
  
  <details>

  ```
  Run the command with updated image tag:
  kubectl run --restart=Never --image=busybox:1.28.4 static-busybox--dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml
  ```
  </details>

- Identify which node the static pod is created on, ssh to the node and delete the pod definition file. If you don't know theIP of the node, run the kubectl get nodes -o wide command and identify the IP. Then SSH to the node using that IP. For static pod manifest path look at the file /var/lib/kubelet/config.yaml on node01

  <details>

  ```
  $ kubectl get pods -o wide
  $ kubectl get nodes -o wide
  $ ssh <ip address of pod>
  $ grep staticPodPath /var/lib/kubelet/config.yaml
  $ node01 $ rm -rf /etc/just-to-mess-with-you/greenbox.yaml
  ```
  </details>
