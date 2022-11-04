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
> kind apply cluster --name kindmultinodes --config Cluster_create/multinode_cluster.yml <br/>
  kind delete cluster --name=kindmultinodes

