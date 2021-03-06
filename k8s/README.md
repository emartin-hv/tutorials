# Kubernetes

A lot of the info on this page comes from the Kubernetes Cheatsheet: 

https://kubernetes.io/docs/reference/kubectl/cheatsheet/

- Kubernetes Lens: https://k8slens.dev
- K9s: https://k9scli.io

```
brew install derailed/k9s/k9s
```

# Cluster Info

Display addresses and services
```
kubectl cluster-info
```

Dump cluster state

```
kubectl cluster-info dump
```

# Kube Contexts

File that contains kube configs

```
$HOME/.kube/config
```

List the kube context

```
kubectl config current-context
kubectx
```

Change the kube context

```
kubectx contextname
```

Specify a default Kube config

```
kubectl --kubeconfig devops.kubeconfig get all
```

# Nodes

Get info about the nodes
```
# Simple
kubectl get nodes

# Detailed
kubectl get nodes -o wide
```

# Namespaces

List the namespaces

```
kubens get ns

kubens
```

Change the namespace

```
kubens namespace
```

Create/delete a namespace

```
kubectl create namespace new-namespace
kubectl create ns new-namespace

kubectl delete ns new-namespace
```

# kubectl

Install kubectl on Mac

```
brew install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/amd64/kubectl"
```

Install kubectl on Linux

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

Get the version

```
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"16", GitVersion:"v1.16.2", GitCommit:"c97fe5036ef3df2967d086711e6c0c405941e14b", GitTreeState:"clean", BuildDate:"2019-10-15T23:42:50Z", GoVersion:"go1.12.10", Compiler:"gc", Platform:"darwin/amd64"}
```

Get all items

```
# Get all items in the current namespace
kubectl get all

# Get all items in the namespace test
kubectl get all -n test

# Get all items in ALL namespaces
kubectl get all -A
```

Manage a kube config yaml file

```
kubectl apply -f pod_config.yaml
kubectl delete -f pod_config.yaml
kubectl edit -f pod_config.yaml
```

Apply all the kube configs in a folder path

```
kubectl apply -k folder
```

Get the bitnami Docker image

```
docker pull bitnami/kubectl
docker pull bitnami/kubectl:1.17
docker pull bitnami/kubectl:1.18
```

Setup a reverse proxy to securely reach ports on the cluster

```
kubectl proxy

curl http://localhost:8080/api/
```

Port-forward on the local host to a service (8001 -> 8080)

```
kubectl port-forward svc/nginx 8001:8080
```

# kubectx / kubens

Install kubectx/kubens on Mac

```
brew install kubectx
```

Install kubectx/kubens on Linux

```
sudo apt install kubectx

or

sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx
sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens
```

# Pods

List pods

```
# Get all pods in current namespace
kubectl get pods
kubectl get po

# Get all pods in the namespace 'test'
kubectl get po -n test

# Get all pods in ALL namespaces
kubectl get po -A
```

Describe info about a pod 

```
# Text
kubectl describe po pod-name

# Yaml
kubectl describe po pod-name -o yaml
```

Delete a pod

```
kubectl delete po pod-name
```

Get the logs of a pod

```
# Get the logs of a pod
kubectl logs pod-name

# Get the logs of a pod with more than one container
kubectl logs pod-name
```

# Service

Get/Describe a service

```
kubectl get svc service-name
kubectl get svc service-name -n test

kubectl describe svc service-name --namespace=jenkins
```

Port forward local host to service (8000 -> 443)

```
kubectl port-forward -n istio-system svc/istio-ingressgateway 8000:443
```

Delete a service

```
kubectl delete svc service-name
```

# Secrets

Create a generic secret

```
# Singles
kubectl create secret generic my-secret --from-literal=key-id="key"

# Multiples
kubectl create secret generic my-secret --from-literal=key-id="key" --from-literal=access-key="key"
```

# Custom Resource Definitions (CRDs)

Get/Describe CRDs

```
kubectl get crd
kubectl get crd networking.istio.io

kubectl describe crd networking.istio.io
kubectl describe crd networking.istio.io -o yaml
```

# Argo CD

Install Argo CD

```
kubectl create ns argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v1.7.7/manifests/install.yaml
```

Uninstall Argo CD

```
kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v1.7.7/manifests/install.yaml
kubectl delete ns argocd
```

Access ArgoCD on port 8080

```
kubectl port-forward svc/argocd-server -n argocd 8080:443
``` 


Patch Argo CD to Load Balancer or back to ClusterIP:

```
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "ClusterIP"}}'
```

Label a namespace for istio

```
kubectl label namespace argocd istio-injection=enabled
```

# Minikube

Install minikube

```
brew install minikube
```

Version and help

```
minikube version
minikube
```

Main Operations

```
# Start a new minikube
minikube start
minikube start --memory 4096
minikube stop

# Delete the minikube
minikube delete

# Get the IP
minikube ip

# Gets/sets the current minikube profile
minikube profile
```

# nginx

Deploy nginx to cluster

```
# Create a 3 replica nginx deployment
kubectl create deployment nginx --image=nginx --replicas=3

# Create a service for the nginx (8080 -> 80)
kubectl create service nodeport nginx --tcp=8080:80

# Local port forward (8001 -> 80)
kubectl port-forward svc/nginx 8001:8080

# Delete the service
kubectl delete service nginx

# Delete the deployment
kubectl delete deployment nginx 
```

# Tekton

Install pipeline and dashboard
```
 kubectl apply -f https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
 kubectl apply -f https://storage.googleapis.com/tekton-releases/dashboard/latest/tekton-dashboard-release.yaml
```

Expose the Tekton dashboard
```
kubectl --namespace tekton-pipelines port-forward svc/tekton-dashboard 9097:9097
```