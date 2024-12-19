# Instalação do Kubectl

`$ curl -LO https://dl.k8s.io/release/v1.24.2/bin/linux/amd64/kubectl`

`$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl`

`$ kubectl version --client --output=yaml`

- Output

- ```
  clientVersion:
   buildDate: "2022-06-15T14:22:29Z"
   compiler: gc
   gitCommit: f66044f4361b9f1f96f0053dd46cb7dce5e990a8
   gitTreeState: clean
   gitVersion: v1.24.2
   goVersion: go1.18.3
   major: "1"
   minor: "24"
   platform: linux/amd64
  kustomizeVersion: v4.5.4
  ```

# Instalação MiniKube

`$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/v1.26.0/minikube-linux-amd64`

`chmod +x minikube`

`sudo install minikube /usr/local/bin`

# Iniciando MiniKube

`minikube delete`

`minikube start`

- Output

```
» minikube start
😄  minikube v1.26.0 on Ubuntu 22.04
✨  Automatically selected the docker driver. Other choices: none, ssh
📌  Using Docker driver with root privileges
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🎉  minikube 1.34.0 is available! Download it: https://github.com/kubernetes/minikube/releases/tag/v1.34.0
💡  To disable this notice, run: 'minikube config set WantUpdateNotification false'

💾  Downloading Kubernetes v1.24.1 preload ...
    > preloaded-images-k8s-v18-v1...: 405.83 MiB / 405.83 MiB  100.00% 29.69 Mi
    > gcr.io/k8s-minikube/kicbase: 385.99 MiB / 386.00 MiB  100.00% 5.29 MiB p/
    > gcr.io/k8s-minikube/kicbase: 0 B [_________________________] ?% ? p/s 47s
🔥  Creating docker container (CPUs=2, Memory=3900MB) ...
🐳  Preparing Kubernetes v1.24.1 on Docker 20.10.17 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

```

`minikube status`

- Obtendo os pods
  `$ kubectl get pods`
- Obtendo todos os pods
  `$ kubectl get pods --all-namespaces`
  Output

```
» kubectl get pods --all-namespaces
NAMESPACE     NAME                               READY   STATUS    RESTARTS        AGE
kube-system   coredns-6d4b75cb6d-9mkb6           1/1     Running   0               4h18m
kube-system   etcd-minikube                      1/1     Running   0               4h19m
kube-system   kube-apiserver-minikube            1/1     Running   0               4h19m
kube-system   kube-controller-manager-minikube   1/1     Running   0               4h19m
kube-system   kube-proxy-584bd                   1/1     Running   0               4h18m
kube-system   kube-scheduler-minikube            1/1     Running   0               4h19m
kube-system   storage-provisioner                1/1     Running   1 (4h18m ago)   4h19m

```

$ kubectl run <docker_image_container> --image httpd

$ kubectl get pods -o wide

$ kubectl get pods --all-namespaces

$ kubectl delete pods <pod_name>

- Criando atraves do arquivo manifesto yaml
  $ kubectl create -f arquivo-name.yaml

» kubectl create -f pod.yaml
pod/my-pod created
gabriel in github/kubernetes.former.course/kubernetes …
» kubectl get pods  
NAME READY STATUS RESTARTS AGE
my-pod 0/1 ContainerCreating 0 4s

- Alternativa do create
  $ kubectl apply -f pod.yaml

$ kubectl get replicasets

» kubectl create -f replicaset.yaml
replicaset.apps/frontend-rs created
gabriel in github/kubernetes.former.course/kubernetes …
» kubectl get replicasets  
NAME DESIRED CURRENT READY AGE
frontend-rs 4 4 4 10s
gabriel in github/kubernetes.former.course/kubernetes …
» kubectl get pods  
NAME READY STATUS RESTARTS AGE
frontend-rs-cbw4x 1/1 Running 0 21s
frontend-rs-pmnn6 1/1 Running 0 21s
frontend-rs-v49lc 1/1 Running 0 21s
frontend-rs-zltgr 1/1 Running 0 21s
gabriel in github/kubernetes.former.course/kubernetes …

$ kubectl delete --all pods
$ kubectl delete --all pods && kubectl get pods
$ kubectl delete replicasets <nome_da_replica>
