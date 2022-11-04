Below is the Kubernetes architecture

![Alt text](images/Kubernetes-architecture.png?raw=true "Kubernetes Architecture")
Image: Source Internet


**ETCD** 
> Is a database of kubernetes and  store value in key-pair format.<br />
A consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.

**Kube API SERVER**  
> The Kubernetes API server authenticate and validates request. <br />
The Kubernetes API server validates and configures data for the api objects which include pods, services, replicationcontrollers, and others.


**Kube Controller Manager** 
> Continues monitor controller Manage status with help of kube api server and update status.<br/>
Node monitor period is 5s and Grace period is 40s.<br/>

**Kube Scheduler**
> responsible for scheduling pod's and node's. it decide which pod goes to which node.

**Kubelet**
> The kubelet is the primary "node agent" that runs on each node.

**Kube Proxy**
> is kubernetes network to communicate pods internally. 

**Kube Controller Manager**