New Exam Preparation Practice Test
----------------------------------------------

Solution 1 - Cluster Role & Service Account

-----------------------------------------------------

Step 1: Create Namespace

kubectl create namespace demo-namespace

Step 2: Create Service Account in Custom Namespace

apiVersion: v1
kind: ServiceAccount
metadata:
  name: demo-token
  namespace: demo-namespace
Step 3:  Create a Cluster Role

kubectl create clusterrole demo-clusterrole --verb=create --resource=Deployment,StatefulSet,DaemonSet --dry-run=client -o yaml > clusterrole.yaml

Following is the snippet generated from the above command:

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  creationTimestamp: null
  name: demo-clusterrole
rules:
- apiGroups:
  - apps
  resources:
  - deployments
  - statefulsets
  - daemonsets
  verbs:
  - create
kubectl apply -f clusterrole.yaml

Step 4: Bind the ClusterRole with Service Account

kubectl create clusterrolebinding demo-role-bind --clusterrole=demo-clusterrole --serviceaccount=demo-namespace:demo-token

The following snippet is generated from the above code:

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  creationTimestamp: null
  name: demo-role-bind
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: demo-clusterrole
subjects:
- kind: ServiceAccount
  name: demo-token
  namespace: demo-namespace


----------------------------------------------

Solution 2 - Draining Node

-----------------------------------------------------

Step 1: Find in which node the test-pd POD is running

kubectl get pods -o wide

Step 2: Making that node unavailable and rescheduling the pods

kubectl drain k8s-worker-nodes-3pohh --delete-local-data --ignore-daemonsets --force



----------------------------------------------

Solution 3 - Kubeadm installation

-----------------------------------------------------

Follow the steps mentioned in the following page to install kubeadm of 1.18 version

https://github.com/zealvora/certified-kubernetes-administrator/tree/master/Domain%206%20-%20Cluster%20Architecture,%20Installation%20&%20Configuration



--------------------------------------------------

Solution 4 - Kubeadm Cluster Upgrade

-----------------------------------------------------

apt-mark unhold kubelet kubectl
apt-get install -qy kubeadm=1.19.1-00
kubectl drain kubeadm-master --ignore-daemonsets
kubeadm upgrade plan
kubeadm upgrade apply v1.19.0
kubectl uncordon kubeadm-master


--------------------------------------------------

Solution 5 - ETCD Backup & Restore

-----------------------------------------------------

Step 1: Add Data to cluster:

ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/etcd/ca.crt --cert=/etc/etcd/etcd.crt --key=/etc/etcd/etcd.key put course "I am awesome"

Step 2: Take Backup of ETCD Cluster

ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/etcd/ca.crt --cert=/etc/etcd/etcd.crt --key=/etc/etcd/etcd.key snapshot save /tmp/firstbackup.db

Step 3: Add Data to Cluster

ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/etcd/ca.crt --cert=/etc/etcd/etcd.crt --key=/etc/etcd/etcd.key put course "We are awesome"

Step 4: Restore from Snapshot

systemctl stop etcd
rm -rf /var/lib/etcd/*
cd /tmp 
 
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/etcd/ca.crt --cert=/etc/etcd/etcd.crt --key=/etc/etcd/etcd.key snapshot restore firstbackup.db
 
mv /tmp/default.etcd/* /var/lib/etcd
Step 5: Create etcd user

useradd etcd

Step 6: Add permissions for etcd user to the path of restore

chown -R etcd.etcd /var/lib/etcd

Step 6: Start etcd

systemctl start etcd
Step 7: Verify if "I am awesome" data is present

ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/etcd/ca.crt --cert=/etc/etcd/etcd.crt --key=/etc/etcd/etcd.key get course



--------------------------------------------------

Solution 6 - Network Policy

-----------------------------------------------------

Step 1: Create a custom namespace

kubectl create namespace custom-namespace

Step 2: Create POD in  the namespace

kubectl run nginx --image=nginx --namespace custom-namespace

Step 3: Add a Network Policy According to Requirement

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-network-policy
  namespace: custom-namespace
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector: {}
    ports:
    - protocol: TCP
      port:  80
--------------------------------------------------

Solution 7 - Scaling Deployments

-----------------------------------------------------

Step 1: Create Deployment

kubectl create deployment my-deployment --image=nginx

Step 2: Scale Deployment

kubectl scale --replicas 5 deployment/my-deployment



--------------------------------------------------

Solution 8 - Multi-Container PODS

-----------------------------------------------------



apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pods
spec:
  containers:
  - name: nginx
    image: nginx
  - name: redis
    image: redis


--------------------------------------------------

Solution 9 - Persistent Volumes - Host Path

-----------------------------------------------------

Step 1: Create PV according to specification

apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 2Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "data"
Step 2: Mount PV in POD

apiVersion: v1
kind: Pod
metadata:
  name: pv-pod
spec:
  containers:
  - image: nginx
    name: test-container
    volumeMounts:
    - mountPath: /mydata
      name: my-pv
  volumes:
  - name: my-pv


--------------------------------------------------

Solution 10 - Ingress

-----------------------------------------------------

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: demo-ingress
  namespace: demo-namespace
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /web
        pathType: Prefix
        backend:
          service:
            name: web
            port:
              number: 8080
--------------------------------------------------

Solution 11 -  POD Logging

-----------------------------------------------------

Step 1: Create Objects based on URL

kubectl apply -f https://raw.githubusercontent.com/zealvora/myrepo/master/demo-files/cka_logs.yaml

Step 2: Write Specific Logs to a file

kubectl logs counter2 --all-containers | grep 02 > /opt/kplabs-foobar.txt



--------------------------------------------------

Solution 12 -  Node Selector

-----------------------------------------------------

apiVersion: v1
kind: Pod
metadata:
  name: selector-pod
spec:
  containers:
  - name: nginx
    image: nginx
  nodeSelector:
    disk: ssd


--------------------------------------------------

Solution 13 - Deployment & Metric Server

-----------------------------------------------------

Step 1: Create Deployment

apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: overload-pods
  name: overload-pods
spec:
  replicas: 3
  selector:
    matchLabels:
      app: overload-pods
  template:
    metadata:
      labels:
        app: overload-pods
    spec:
      containers:
      - image: nginx
        name: nginx
 
Step 2: Find Pods with Highest CPU and store it in a file

kubectl top pods -l name=overload-pods > /tmp/overload.txt



--------------------------------------------------

Solution 14 - Deployment & NodePort

-----------------------------------------------------

Step 1 - Create the deployment

apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: service-deployment
  name: service-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: service-deployment
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: service-deployment
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
Step 2 - Edit the Deployment and add a Named port under spec

kubectl edit deployment service-deployment




Step 3 - Create Service According to Specification

kubectl expose deployment service-deployment --name node-port-service --port=80 --target-port=http --type=NodePort

