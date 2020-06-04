<h1 align="center">
  <br>
  <a><img src="images/logo.png" height="200" widht="150"></a>
  <br>
  Kubernetes Cheat Sheet
  <br>
</h1>
It is an open source system which helps in creating and managing containerization of application.
It allows you to deploy and manage multi-container applications at scale. While in practice Kubernetes is most often used with Docker, the most popular containerization platform.</br> 
This tutorial provides an overview of different kind of features and functionalities of Kubernetes.


## Table of Contents
* [What is Container Orchestration](#what-is-container-orchestration)
* [Why do we need Container Orchestration](#why-do-we-need-container-orchestration)
* [Prerequisites](#Prerequisites)
* [Installation](#Installation)


## What is Container Orchestration
Container orchestration is the automated arrangement, coordination and management of containers in their clusters.</br>
Its is an automation of all aspects of coordinating and managing containers.</br>
Kubernetes provides features of auto-replication, auto-scaling, auto-healing, volume management, networking.


## Why do we need Container Orchestration
Container orchestration is used to automate the following task in individual containers among the mircoservice ecosystem:
* High Availablity of Containers
* Configuration and Scheduling of Containers
* Dynamic allocation of resources among Containers
* Autoscaling of Containers
* Load balancing, traffic routing and service discovery of Containers
* Securing communication in between the Containers


## Prerequisites
One who wants to understand Kubernetes should have an understating of how the Docker works, how the Docker images are created, and how they work as a standalone unit. it would help if the readers have some exposure to Linux.


## Installation
To work on kubernetes, Docker must be install on the machine. This installation is done targeting Windows OS.</br>
The following steps for Installation:

* Download and Install Docker Desktop for Windows [Installation Guide](https://docs.docker.com/docker-for-windows/install/)
* Start Docker Desktop 
* Start Kubernetes by enabling Kubernetes

 <a><img src="images/dd_setting.png"></a>

### Download Kubectl for Windows
Run below command from windows powershell
```
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.18.0/bin/windows/amd64/kubectl.exe
```
It will download a kubectl.exe file place it any where on your machine directory.
Add the binary in to your PATH.(Env Variable)

Test to ensure the version of kubectl is the same as downloaded:
```
$ kubectl version --client
```


### Kuberneter Dashboard
Create File user.yml 
```
$ nodepad user.yml
```
Paste the content below 
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-system
```
To apply the changes:
```
kubectl apply -f user.yml
```


Create role.yml
```
$ nodepad role.yml
```
Paste the content below
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kube-system
```

To apply the changes:
```
kubectl apply -f role.yml
```
To get system Kubectl Secret Token 
```
$ kubectl -n kube-system get secret | findstr admin-user
```

To get the token password to login Kubernetes Dashboard
```
 $ kubectl -n kube-system describe secret <Token from above command>
```
> Copy and keep the output token from above command safe in text file for logging into Dashboard

To Download Dashboard
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml
```

If you like charts and graphs so also add these as well:
```
$ kubectl create -f https://raw.githubusercontent.com/kubernetes/heapster/master/deploy/kube-config/influxdb/influxdb.yaml
$ kubectl create -f https://raw.githubusercontent.com/kubernetes/heapster/master/deploy/kube-config/influxdb/heapster.yaml
$ kubectl create -f https://raw.githubusercontent.com/kubernetes/heapster/master/deploy/kube-config/influxdb/grafana.yaml
```

Start K8s Dashboard
```
$ kubectl proxy
```

Open the Link below: </br>
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login
> Use the saved token to login

<a><img src="images/kubernetes_login.png"></a>

### Kuberneters Dashboard
<a><img src="images/kubernetes_home.png"></a>
