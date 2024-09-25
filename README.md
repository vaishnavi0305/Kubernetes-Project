**Kubernetes Pods:-**

In Docker you directly deploy container, but in kubernetes you cannot directly deploy container, you will need to deploy container using Pod.
Pod is the smallest unit of deployment in kubernetes.

It is like a wrapper around the container, Mostly it is recommended to use 1 container per pod but you may use more than 1 container in single pod in some cases like side car or others

pods is used to make the life of a devops engineer easy as it uses declarative language and you will define the container and its version inside the pods.yaml file
You can understand pods as definition of How to run a container.

For understanding, to run a container in docker you will build the image and to do that you will enter the below command , means you will pass the name of the container, ports volume, network information in the command line, but for it to be declarative in nature, we will create a pod.yaml file and enter all of the specification of the container inside it.

_docker run -d nameof the container -p ..... --network ...-v ......._

Pod will be assigned with the cluster ip address and container will also be using or assigned the same IP address as pod. And cluster Ip address is provided by kubeproxy.
Pod

**Kubectl:-**
same as in Docker, if you want to run something inside docker like docker ps, docker build, docker run you use docker cli.
Similary in Kubernetes, there is Kubectl. (Command line toolfor Kubernetes)
Example:- if you have kubernetes cluster and there are 10 nodes inside it , so to understand or get that information you will use

_kubectl get nodes_

**Handson to Deploy first pod in Kubernetes:-**

**step1 :- install kubectl**

(Note:- before installing make sure you check the architecture of your machine is it x86 or arm64, you can use the command “uname -m”)

>curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
>Kubectl version // to check the version of kubectl installed

ref:- https://kubernetes.io/do cs/tasks/tools/install-kubectl-linux/

**step2:- Install minikube**

(To setup local kubernetes cluster for demo, as running the kuberntes using KOPS will need you to get charged on AWS resources, so for demo we can use the local kubernetes cluster)

>curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
>sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

Ref:- https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download

> minikube //To verify if minikube is installed.

(Minikube is a command line tool which will help you in creating a kubernetes cluster)

**step 3:- create a minikube or kubernetes cluster**

>Understand how minikube creates a kubernetes cluster:-
It will create a VM first and then it will create a single node kubernetes cluster.
(Note:- in production or in realtime scenario, we will use the multi node kubernetes cluster i.e in general there will be 3 master nodes and n number of worker nodes for high availability and complexity)
>Minikube will by default use the docker driver, but it is not recommended when performing complex kubernetes task.
> minikube start // it will create the kubernetes cluster using the default docker driver, but in realtime or for complex kubernets concept it is not recommended, so you can use the default virtualization platform in linux i.e KVM, or virtualbox. In our case we have used virtual box

Output:-

$ minikube start
😄  minikube v1.34.0 on Ubuntu 24.04 (xen/amd64)
✨  Automatically selected the docker driver. Other choices: ssh, none

⛔  Exiting due to RSRC_INSUFFICIENT_CORES:  has less than 2 CPUs available, but Kubernetes requires at least 2 to be available

-----------------------
So you will need to use the linux supported hypervisor or virtualization platform
follow the steps mentioned below

>minikube start –memory=4096 -driver=virtualbox

![image](https://github.com/user-attachments/assets/1cda59d2-0c64-4df5-9044-f1301d9e2eec)

Now run Kubectl get nodes to get node information running on your kubernetes cluster.
![image](https://github.com/user-attachments/assets/a88f5d28-ed71-4f57-be31-d08a3f10f8db)


Now that you have Minikube installed, cluster created and nodes are up and running , we can now create pods.

So we will create a pod which will run the nginx application, below is the pod.yml file

apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
- containerPort: 80

Then will create the pod using 
>kubectl create -f pod.yml

To check if the pods are created we will do
>kubectl get pods
or to get more information about pod you can use 
>kubectl get pods -o wide

![image](https://github.com/user-attachments/assets/8ab1d6c5-fd7c-437d-abcc-0f333c402277)

Now to check if the application is running or not, you can ssh into minikube and curl onto the pods ip address

minikube ssh
curl ipaddressOfPod

![image](https://github.com/user-attachments/assets/03ef76f3-003d-4909-a38f-12cbf1f0eb00)
 
Hurray, Here we have successfully deployed our first app on kubernetes!!
