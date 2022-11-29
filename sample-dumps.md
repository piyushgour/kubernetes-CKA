Exam Preparation Practice Test
Question 1: ClusterRole and ClusterRole Binding

Create a new ClusterRole named demo-clusterrole. Any resource associated with the cluster role should be able to create the following resources:

Deployment StatefulSet DaemonSet

Create a new namespace named demo-namespace. Within that namespace, create a new ServiceAccount named demo-token. Bind the new ClusterRole with the custom service-account token

Limited to namespace demo-namespace, bind the new ClusterRole demo-clusterrole to the new ServiceAccount demo-token.

Question 2: Drain Nodes

Before proceeding, make sure you have two worker node based Kubernetes cluster.

Apply the following Manifest file:

kubectl apply -f https://raw.githubusercontent.com/zealvora/myrepo/master/demo-files/drain.yaml

2 Requirements:

- Find out in which node a pod named test-pd is running.

- Make that node unavailable and reschedule all pods on that node.

Question 3: KUBEADM Cluster Initialization

Create a new kubeadm based K8s cluster. The cluster should be based on the 1.18.0 Kubernetes version. All the components including kubelet, kubectl should also be on the same version. The cluster should be based on Ubuntu OS.

Question 4: KUBEADM Cluster Upgrade

Upgrade the kubeadm cluster created in the above step from 1.18.0 to 1.19.0 version only. In addition, also make sure that kubelet, and kubectl are upgraded to the same version as kubeadm.  Ensure to drain the master node before upgrading and uncordon the master node after upgrading. No other worker nodes should be upgraded.

Question 5: ETCD Backup & Restore

For this question, you should have a working ETCD cluster with TLS certificates. You can refer to the following guide to create an ETCD cluster:

https://github.com/zealvora/certified-kubernetes-administrator/blob/master/Domain%206%20-%20Cluster%20Architecture%2C%20Installation%20%26%20Configuration/k8s-scratch-step-3-install-etcd.md

Requirements:

i). Add the following data to the ETCD cluster:

course "I am awesome"

ii). Take a backup of the ETCD cluster. Store the backup at /tmp/firstbackup.db path

iii). Add one more data to the ETCD cluster

course "We am awesome"

iv) Restore the backup from /tmp/firstbackup.db to ETCD

v) Create etcd user with useradd etcd command

v) Make sure that whichever directory ETCD backup data is restored has full permissions for the etcd user.

vi) Verify if "I am awesome" data is present"

Question 6: Network Policies

Create a new namespace named custom-namespace.

Create a new network policy named my-network-policy in the custom-namespace.

Requirements:

i) Network Policy should allow PODS within the custom-namespace to connect to each other only on Port 80. No other ports should be allowed.
 
ii) No PODs from outside of the custom-namespace should be able to connect to any pods inside the custom-namespace.


Question 7: Scale Deployments

Create a new deployment named my-deployment based on nginx image. Deployment should have 1 replicas.

Scale the deployment to 5 replicas.

Question 8: Multi-Container Pods

Create a pod named multi-container. This POD should have two containers

nginx + redis

Question 9: Persistent Volume

Create a Persistent Volume with the following specification:

Name: my-pv
Hostpath of /data should be mounted inside POD on /mydata location
Storage: 2Gi
Question 10: Ingress

Create a new ingress resource based on the following specification:

Name: demo-ingress
Namespace: demo-namespace
Expose service web on the path of /web using the serviceport of 8080
Question 11: POD and Logging

Apply the following manifest to your K8s cluster:

https://raw.githubusercontent.com/zealvora/myrepo/master/demo-files/cka_logs.yaml

Monitor the logs for all the containers which are part of the counter2 pod. Extract log lines which have the number 02. Write them to /opt/kplabs-foobar.txt



Question 12: NodeSelector

Create a pod named selector-pod based on nginx image. The POD should only run on the node which has a label of disk=ssd

Question 13: Metric Server

Note: For Exams, Metric Server will be pre-installed. You do not have to install it.

Create deployment named overload-pods based on nginx image. There should be 3 replicas.

Create deployment named underload-pods based on nginx image. There should be 2 replicas.

For the overload-pods deployment, find the PODS that are taking the highest CPU and store data to /tmp/overload.txt

Question 14: Service and Deployment

i) Create deployment named service-deployment based on nginx image with 3 replicas.

ii) Re-configure the existing deployment and add named port http to expose 80/tcp port of containers.

iii) Create a new service named node-port-service to expose the http port. This service should be based on NodePort.