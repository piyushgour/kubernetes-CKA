## Prerequisites
> OS: Windows 10 version 2004, Build 19041<br/>
 WSL2 enabled

## Try to see if the docker cli and kubectl are installed
> docker version <br/>
  kubectl version

## Download the latest version of KinD
> curl -Lo ./kind https://github.com/kubernetes-sigs/kind/releases/download/v0.7.0/kind-linux-amd64 <br/>
(Make the binary executable)<br/>
chmod +x ./kind <br/>
(Move the binary to your executable path) <br/>
sudo mv ./kind /usr/local/bin/ 



## Now Create Cluster 
> Create the cluster and give it a name (optional)<br/>
    kind create cluster --name testkind <br/>
    (Check if the .kube has been created and populated with files) <br />
    ls $HOME/.kube

##  cluster was created and it's the "normal" one node cluster:
> kubectl get nodes <br/>
  kubectl get all --all-namespaces

## Delete the existing cluster
> kind delete cluster --name testkind

## Multinode cluster
> kind create cluster --name kindmultinodes --config Cluster_create/multinode_cluster.yml <br/>
  kind delete cluster --name=kindmultinodes



## Cluster UI configuration

>  Install the Dashboard application into our cluster <br/>
 kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-rc6/aio/deploy/recommended.yaml <br/>
 Check the resources it created based on the new namespace created <br/>
 kubectl get all -n kubernetes-dashboard
 <br/>

### Now Access Dashboard
>  Start a kubectl proxy<br/>
- kubectl proxy 
 Enter the URL on your browser:<br/>
  http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

#### Now create RBAC and it's Token
> (Create a new ServiceAccount)<br/>
 kubectl apply -f - service_account_for_admin_user.yml <br/>
 (Create a ClusterRoleBinding for the ServiceAccount) <br/>
 kubectl apply -f clusterRoleBinding.yml<br/>

#### Get the Token for the ServiceAccount) <br/>
- kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}') <br/>
(Copy the token and copy it into the Dashboard login and press "Sign in")

<br/><br/><br/>
Ref- https://kubernetes.io/blog/2020/05/21/wsl-docker-kubernetes-on-the-windows-desktop/