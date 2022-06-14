# k8s-101

# What are key k8s cluster components
```
# kubectl get componentstatuses
Warning: v1 ComponentStatus is deprecated in v1.19+
NAME                 STATUS    MESSAGE                         ERROR
scheduler            Healthy   ok
controller-manager   Healthy   ok
etcd-0               Healthy   {"health":"true","reason":""}
```
![k8s components](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)
K8s cluster consists of
ControlPlane 
@ Master Node: VM(s) or HW Server(s) that runs Control Plane
1) kube-API Server
2) kube-Scheduler (to put pods on worker nodes aka binpack) 
3) kube-Controller Manager
4) Cloud Control manager (runs elsewhere?)
5) etcd (consistent, high-available key-value store) 
Application runtimes
@ Worker/Minion Node(s)  - VM(s) or HW Server(s) that runs pods.
6) Kubelet
7) Kube proxy
8) Container runtime

[video, ru](https://www.youtube.com/watch?v=q_nj340pkQo&list=PLg5SS_4L6LYvN1RqaVesof8KAf-02fJSi)

# How to start experimenting with k8s in 2022?
- download kubectl, kubeadm and minikube binaries 
- install docker deamon
```
#minikube start --driver=docker # or --driver=virtualbox
```
then 
```
#kubectl version
#kubectl get pods -A
#minikube dashboard
#kubectl cluster-info #dump
#kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
#kubectl expose deployment hello-minikube --type=NodePort --port=8080
#kubectl port-forward service/hello-minikube 7080:8080
#minikube dashboard
```

### Top 20 Kubernetes Interview Questions
([source](https://medium.com/@croguerrero/top-20-kubernetes-interview-questions-e2fe0dcc6c52) (c) Christian Guerrero)

Kubernetes is the greatest orchestration technology on the market right now, making this open-source system one of the most in-demand options. Today, Kubernetes is used by several international corporations, including Yahoo, SAP, SoundCloud, Huawei, and eBay.

1. What is software/DevOps orchestration?
Orchestration integrates various apps or services to automate a process or synchronize data in real-time. Orchestration allows microservices in distinct containers to interact smoothly to accomplish a purpose.

2. How does Kubernetes orchestrate?
Kubernetes simplifies DevOps activities, including deployment, scalability, and setup. Containers host most distributed apps. App microservices may operate together in a container. Externally managed containers must be scheduled, distributed, and load-balanced to support infrastructure. Data persistence and network settings make clustered container management difficult.

3. What are Kubernetes’ benefits?
Kubernetes simplifies container management. It speeds up application deployment, improving customer service. Kubernetes automates rollbacks and scheduling. Kubernetes can cluster hosts across private, public, and hybrid clouds, making it perfect for cloud-native programs demanding quick scalability.

4. What are Kubernetes namespaces?
Kubernetes namespaces split cluster resources across users. This is beneficial when consumers are geographically dispersed.

5. What is a node in Kubernetes?
Nodes are the smallest computational unit. A minion is a single computer in a cluster that executes pods. Kubernetes master components govern the actual or virtual computer.

6. What are Kubernetes’ two primary components?
Master and worker nodes make up Kubernetes’ architecture. The master node includes an API server, scheduler, controller manager, etcd. Each worker node runs container runtime and Kube proxy.

7. What’s a pod’s purpose?
Kubernetes pods hold containers. Each pod may accommodate different containers based on settings. The pod’s containers share resources and a local network, making communication simpler.

8. Where does Kubernetes store cluster data?
Etcd is Kubernetes’ primary data store. Here is cluster data.

9. How are pods and jobs different in Kubernetes?
Their functions vary. Pods run containers. A task ensures pods run to completion.

10. Describe a kubelet.
Kubelet is Kubernetes’ lowest-level component. It ensures that all containers in a pod are functioning during their lifespan.

11. What’s kube-proxy?
Kube-proxy handles host sub-netting and provides system services. It directs incoming requests depending on IP and port number, aiding with load balancing.

12. What is Kubernetes Heapster?
Kubernetes’ Heapster monitors performance. It gathers metrics on all nodes like another cluster pod. It’s Kubernetes-native.

13. What’s a Kubernetes scheduler?
Kube-scheduler distributes workload across worker nodes. It schedules jobs and monitors resource utilization to ensure proper allocation.

14. What are Daemon sets?
Daemon sets are one-time-use pods. They’re utilized for host layer properties like network monitoring, which execute once per host.

15. How do you ensure a pod always runs?
Liveness probes can determine this. It checks whether a pod’s app is running. Failure restarts the container.

16. How can Kubernetes APIs be secured?
Kubernetes API security approaches include:

Use the correct authorization mode with the API server
Use API authentication
Ensure that TLS protects all incoming traffic
Use authorization-mode=Webhook to make kubeless protect the API
Use restrictive RBAC role policy on the kube-dashboard
Remove any default service account permissions
17. What is a Load Balancer?

Load balancing enables network services. Internal load balancing means automatically balancing and assigning pod loads. External load balancing directs traffic to backend pods.

18. What is a controller manager?
Kubernetes controller manager is a daemon that embeds system control loops. Multiple Master node processes assist.

19. What are Google Kubernetes Engine’s uses?
GKE manages Docker containers and clusters. GKE orchestrates Google’s public cloud container clusters.

20. Kubernetes vs Docker Swarm?

Both methods divide applications into containers, allowing for easy application administration and scalability automation. Here is a broad breakdown of their distinctions:

Kubernetes is an open-source and modular orchestration framework that provides an efficient container orchestration solution for high-demand applications with extensive setup.
Docker Swarm prioritizes usability, making it best suited for basic applications that are fast to install and maintain.
Additional resource:https://www.bmc.com/blogs/kubernetes-vs-docker-swarm/
