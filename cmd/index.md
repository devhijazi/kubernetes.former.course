# Tutorial: Configuração de Kubernetes

Este guia passo a passo ensina como instalar e configurar o Kubectl e o Minikube, bem como executar comandos básicos no Kubernetes.

---

## Instalação do Kubectl

1. Baixe o binário do Kubectl:

   ```bash
   $ curl -LO https://dl.k8s.io/release/v1.24.2/bin/linux/amd64/kubectl
   ```

2. Instale o Kubectl:

   ```bash
   $ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
   ```

3. Verifique a versão instalada:

   ```bash
   $ kubectl version --client --output=yaml
   ```

   **Exemplo de Saída:**

   ```yaml
   clientVersion:
     buildDate: '2022-06-15T14:22:29Z'
     compiler: gc
     gitCommit: f66044f4361b9f1f96f0053dd46cb7dce5e990a8
     gitTreeState: clean
     gitVersion: v1.24.2
     goVersion: go1.18.3
     major: '1'
     minor: '24'
     platform: linux/amd64
   kustomizeVersion: v4.5.4
   ```

---

## Instalação do Minikube

1. Baixe o binário do Minikube:

   ```bash
   $ curl -Lo minikube https://storage.googleapis.com/minikube/releases/v1.26.0/minikube-linux-amd64
   ```

2. Torne o binário executável:

   ```bash
   $ chmod +x minikube
   ```

3. Instale o Minikube:
   ```bash
   $ sudo install minikube /usr/local/bin
   ```

---

## Iniciando o Minikube

1. Exclua quaisquer configurações anteriores:

   ```bash
   $ minikube delete
   ```

2. Inicie o cluster Minikube:

   ```bash
   $ minikube start
   ```

   **Exemplo de Saída:**

   ```
   » minikube start
   😄  minikube v1.26.0 on Ubuntu 22.04
   ✨  Automatically selected the docker driver. Other choices: none, ssh
   📌  Using Docker driver with root privileges
   👍  Starting control plane node minikube in cluster minikube
   🚜  Pulling base image ...
   🎉  minikube 1.34.0 is available! Download it: https://github.com/kubernetes/minikube/releases/tag/v1.34.0
   ✡  To disable this notice, run: 'minikube config set WantUpdateNotification false'

   💾  Downloading Kubernetes v1.24.1 preload ...
       > preloaded-images-k8s-v18-v1...: 405.83 MiB / 405.83 MiB  100.00% 29.69 MiB/s
       > gcr.io/k8s-minikube/kicbase: 385.99 MiB / 386.00 MiB  100.00% 5.29 MiB/s
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

3. Verifique o status do Minikube:
   ```bash
   $ minikube status
   ```

---

## Comandos Básicos do Kubernetes

### Gerenciamento de Pods

- Liste os pods:

  ```bash
  $ kubectl get pods
  ```

- Liste todos os pods (em todos os namespaces):

  ```bash
  $ kubectl get pods --all-namespaces
  ```

  **Exemplo de Saída:**

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

- Execute um container:

  ```bash
  $ kubectl run <docker_image_container> --image=httpd
  ```

- Obtenha detalhes dos pods:

  ```bash
  $ kubectl get pods -o wide
  ```

- Exclua um pod:
  ```bash
  $ kubectl delete pods <pod_name>
  ```

### Usando Manifestos YAML

- Crie recursos com um arquivo YAML:

  ```bash
  $ kubectl create -f arquivo-name.yaml
  ```

  **Exemplo de Saída:**

  ```
  » kubectl create -f pod.yaml
  pod/my-pod created
  » kubectl get pods
  NAME       READY   STATUS              RESTARTS   AGE
  my-pod     0/1     ContainerCreating   0          4s
  ```

- Alternativa ao `create`:
  ```bash
  $ kubectl apply -f pod.yaml
  ```

### Gerenciamento de ReplicaSets

- Liste os ReplicaSets:

  ```bash
  $ kubectl get replicasets
  ```

- Crie um ReplicaSet:

  ```bash
  $ kubectl create -f replicaset.yaml
  ```

  **Exemplo de Saída:**

  ```
  » kubectl get replicasets
  NAME          DESIRED   CURRENT   READY   AGE
  frontend-rs   4         4         4       10s
  ```

- Escale replicas:

  ```bash
  $ kubectl scale replicasets <nome_da_replica> --replicas=numero
  ```

- Exclua todos os pods e ReplicaSets:
  ```bash
  $ kubectl delete --all pods
  $ kubectl delete replicasets <nome_da_replica>
  ```

### Monitoramento

- Monitore pods em tempo real:
  ```bash
  $ watch kubectl get pods
  ```

---

## Gerenciamento de Deployments

- Verifique o status de um Deployment:

  ```bash
  $ kubectl rollout status deployments.apps/<deployment_name>
  ```

- Descreva um Deployment:

  ```bash
  $ kubectl describe deployment.apps/<deployment_name>
  ```

- Consulte o histórico de rollouts:

  ```bash
  $ kubectl rollout history deployment.apps/<deployment_name>
  ```

- Revertê a um rollout:

  ```bash
  $ kubectl rollout undo deployment.apps/<deployment_name>
  ```

- Reverte para uma revisão específica:
  ```bash
  $ kubectl rollout undo deployment.apps/<deployment_name> --to-revision=3
  ```

### Comandos de deleção

```bash
$ kubectl delete all --all --all-namespaces

```

### Teste de comunicação entre PODS

```bash
» kubectl describe pods redis | grep IP
```

```bash
IP:           172.17.0.3
IPs:
  IP:  172.17.0.3
```

```bash
kubectl describe pods tomcat | grep IP
```

```bash
IP:           172.17.0.2
IPs:
  IP:  172.17.0.2
```

```bash
» kubectl exec -it redis -- bash
```

```bash
» kubectl exec -it tomcat -- bash
```
