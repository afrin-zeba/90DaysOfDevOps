## Challenge Tasks

### Task 1: Recall the Kubernetes Story
Before touching a terminal, write down from memory:

1. Why was Kubernetes created? What problem does it solve that Docker alone cannot?
kubernetes acts as a container orchestration tool which allows autoscaling and disaster recovery in case of cluster of containers 
2. Who created Kubernetes and what was it inspired by?
google , inspired by theur tool borg
3. What does the name "Kubernetes" mean?
Kubernetes comes from the Greek word "κυβερνήτης" (kybernetes), meaning "helmsman" or "ship's captain" - someone who steers a ship.

Do not look anything up yet. Write what you remember from the session, then verify against the official docs.

---

### Task 2: Draw the Kubernetes Architecture
From memory, draw or describe the Kubernetes architecture. Your diagram should include:

              Kubernetes Cluster
+----------------------------------------------+
|                                              |
|  Control Plane          Worker Nodes         |
|                                              |
|  Scheduler          ->  kubelet             |
|  API Server         ->  kube-proxy          |
|  Controller Manager ->  Pods (Containers)   |
|  etcd               ->  Container Runtime   |
|                                              |
+----------------------------------------------+

**Control Plane (Master Node):**
- API Server — the front door to the cluster, every command goes through it
- etcd — the database that stores all cluster state
- Scheduler — decides which node a new pod should run on
- Controller Manager — watches the cluster and makes sure the desired state matches reality

**Worker Node:**
- kubelet — the agent on each node that talks to the API server and manages pods
- kube-proxy — handles networking rules so pods can communicate
- Container Runtime — the engine that actually runs containers (containerd, CRI-O)

After drawing, verify your understanding:
- What happens when you run `kubectl apply -f pod.yaml`? Trace the request through each component.
kubectl reads pod.yaml.
It sends the request to the API Server.
The API Server validates the request.
The desired Pod state is stored in etcd.
The Scheduler notices a new unscheduled Pod and selects the best worker node.
The Scheduler updates the Pod with the chosen node via the API Server.
The kubelet on that worker node sees the Pod assignment.
The kubelet asks the container runtime (such as containerd) to pull the image and start the container.
The kubelet reports the Pod status back to the API Server.
The Pod is now running.

- What happens if the API server goes down?
kubectl commands stop working because they communicate with the API Server.
No new Pods, deployments, or updates can be created.
Controllers and the Scheduler cannot function because they also communicate through the API Server.
Existing Pods continue running on worker nodes.
If a Pod crashes, the kubelet may restart its container locally, but Kubernetes cannot make new scheduling or orchestration decisions until the API Server is back.

- What happens if a worker node goes down?
The node stops sending heartbeats to the API Server.
The control plane marks the node as NotReady.
Pods on that node become unavailable.
If those Pods are managed by a Deployment, ReplicaSet, or StatefulSet, Kubernetes creates replacement Pods on healthy worker nodes.
Traffic is redirected to the new Pods once they are running.
---

### Task 3: Install kubectl
`kubectl` is the CLI tool you will use to talk to your Kubernetes cluster.

Install it:
```bash
# macOS
brew install kubectl

# Linux (amd64)
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

# Windows (with chocolatey)
choco install kubernetes-cli
```

Verify:
```bash
kubectl version --client
```

---

### Task 4: Set Up Your Local Cluster
Choose **one** of the following. Both give you a fully functional Kubernetes cluster on your machine.

**Option A: kind (Kubernetes in Docker)**
```bash
# Install kind
# macOS
brew install kind

# Linux
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# Create a cluster
kind create cluster --name devops-cluster
afrinz@Zs-MacBook-Air kube % kind create cluster --name devops-cluster
Creating cluster "devops-cluster" ...
 ✓ Ensuring node image (kindest/node:v1.36.1) 🖼 
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
Set kubectl context to "kind-devops-cluster"
You can now use your cluster with:

kubectl cluster-info --context kind-devops-cluster


# Verify
kubectl cluster-info
afrinz@Zs-MacBook-Air kube % kubectl cluster-info
Kubernetes control plane is running at https://127.0.0.1:59659
CoreDNS is running at https://127.0.0.1:59659/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

kubectl get nodes
afrinz@Zs-MacBook-Air kube % kubectl get nodes
NAME                           STATUS   ROLES           AGE   VERSION
devops-cluster-control-plane   Ready    control-plane   46m   v1.36.1


---

### Task 5: Explore Your Cluster
Now that your cluster is running, explore it:

```bash
# See cluster info
kubectl cluster-info
afrinz@Zs-MacBook-Air kube % kubectl cluster-info
Kubernetes control plane is running at https://127.0.0.1:59659
CoreDNS is running at https://127.0.0.1:59659/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

# List all nodes
kubectl get nodes
afrinz@Zs-MacBook-Air kube % kubectl get nodes
NAME                           STATUS   ROLES           AGE   VERSION
devops-cluster-control-plane   Ready    control-plane   46m   v1.36.1

# Get detailed info about your node
kubectl describe node <node-name>

# List all namespaces
kubectl get namespaces
afrinz@Zs-MacBook-Air kube % kubectl get namespaces
NAME                 STATUS   AGE
default              Active   49m
kube-node-lease      Active   49m
kube-public          Active   49m
kube-system          Active   49m
local-path-storage   Active   49m

# See ALL pods running in the cluster (across all namespaces)
kubectl get pods -A
afrinz@Zs-MacBook-Air kube % kubectl get pods -A
NAMESPACE            NAME                                                   READY   STATUS    RESTARTS   AGE
kube-system          coredns-589f44dc88-j8n9x                               1/1     Running   0          49m
kube-system          coredns-589f44dc88-kc8cl                               1/1     Running   0          49m
kube-system          etcd-devops-cluster-control-plane                      1/1     Running   0          49m
kube-system          kindnet-v5px9                                          1/1     Running   0          49m
kube-system          kube-apiserver-devops-cluster-control-plane            1/1     Running   0          49m
kube-system          kube-controller-manager-devops-cluster-control-plane   1/1     Running   0          49m
kube-system          kube-proxy-mm86z                                       1/1     Running   0          49m
kube-system          kube-scheduler-devops-cluster-control-plane            1/1     Running   0          49m
local-path-storage   local-path-provisioner-855c7b7774-lw6r5                1/1     Running   0          49m


Look at the pods running in the `kube-system` namespace:
```bash
kubectl get pods -n kube-system
afrinz@Zs-MacBook-Air kube % kubectl get pods -n kube-system
NAME                                                   READY   STATUS    RESTARTS   AGE
coredns-589f44dc88-j8n9x                               1/1     Running   0          49m
coredns-589f44dc88-kc8cl                               1/1     Running   0          49m
etcd-devops-cluster-control-plane                      1/1     Running   0          50m
kindnet-v5px9                                          1/1     Running   0          49m
kube-apiserver-devops-cluster-control-plane            1/1     Running   0          50m
kube-controller-manager-devops-cluster-control-plane   1/1     Running   0          50m
kube-proxy-mm86z                                       1/1     Running   0          49m
kube-scheduler-devops-cluster-control-plane            1/1     Running   0          50m

You should see pods like `etcd`, `kube-apiserver`, `kube-scheduler`, `kube-controller-manager`, `coredns`, and `kube-proxy`. These are the architecture components you drew in Task 2 — running as pods inside the cluster.

**Verify:** Can you match each running pod in `kube-system` to a component in your architecture diagram?

---

### Task 6: Practice Cluster Lifecycle
Build muscle memory with cluster operations:

```bash
# Delete your cluster
kind delete cluster --name devops-cluster
# (or: minikube delete)
afrinz@Zs-MacBook-Air kube % kind delete cluster --name devops-cluster
Deleting cluster "devops-cluster" ...
Deleted nodes: ["devops-cluster-control-plane"]

# Recreate it
kind create cluster --name devops-cluster
# (or: minikube start)

# Verify it is back
kubectl get nodes
afrinz@Zs-MacBook-Air kube % kubectl get nodes
NAME                           STATUS   ROLES           AGE   VERSION
devops-cluster-control-plane   Ready    control-plane   29s   v1.36.1
```

Try these useful commands:
```bash
# Check which cluster kubectl is connected to
kubectl config current-context
afrinz@Zs-MacBook-Air kube % kubectl config current-context
kind-devops-cluster

# List all available contexts (clusters)
kubectl config get-contexts
afrinz@Zs-MacBook-Air kube % kubectl config get-contexts
CURRENT   NAME                  CLUSTER               AUTHINFO              NAMESPACE
*         kind-devops-cluster   kind-devops-cluster   kind-devops-cluster   
          kind-udaan-cluster    kind-udaan-cluster    kind-udaan-cluster    

# See the full kubeconfig
kubectl config view
afrinz@Zs-MacBook-Air kube % kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://127.0.0.1:61709
  name: kind-devops-cluster
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://127.0.0.1:58713
  name: kind-udaan-cluster
contexts:
- context:
    cluster: kind-devops-cluster
    user: kind-devops-cluster
  name: kind-devops-cluster
- context:
    cluster: kind-udaan-cluster
    user: kind-udaan-cluster
  name: kind-udaan-cluster
current-context: kind-devops-cluster
kind: Config
users:
- name: kind-devops-cluster
  user:
    client-certificate-data: DATA+OMITTED
    client-key-data: DATA+OMITTED
- name: kind-udaan-cluster
  user:
    client-certificate-data: DATA+OMITTED
    client-key-data: DATA+OMITTED

Write down: What is a kubeconfig? Where is it stored on your machine?
A kubeconfig is a configuration file that tells kubectl which Kubernetes cluster to connect to, how to authenticate, and which context to use.

It stores:
Cluster details (API server URL)
User credentials (certificates, tokens, etc.)
Context (which cluster and user to use by default)
Where is it stored? Linux/macOS: ~/.kube/config
---

## Hints
- kind requires Docker to be running (it creates clusters using containers)
- minikube can use Docker, VirtualBox, or other drivers
- The default kubeconfig file is at `~/.kube/config`
- `kubectl get pods -A` is short for `kubectl get pods --all-namespaces`
- If `kubectl` cannot connect, check if your cluster is running: `kind get clusters` or `minikube status`
- `-o wide` flag gives extra details: `kubectl get nodes -o wide`

---

