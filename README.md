# k8s-101

# Key k8s components
```
#kubectl get componentstatuses
Warning: v1 ComponentStatus is deprecated in v1.19+
NAME                 STATUS    MESSAGE                         ERROR
scheduler            Healthy   ok
controller-manager   Healthy   ok
etcd-0               Healthy   {"health":"true","reason":""}
```
![k8s components](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)
ControlPlane 
@ Master Node: VM(s) or HW Server(s) that runs Control Plane
1) kube-API Server
2) kube-Scheduler (to put pods on worker nodes aka binpack) 
3) kube-Controller Manager
4) Cloud Control manager (runs elsewhere?)
5) etcd (consistent, high-available key-value store) 

@ Worker/Minion Node(s)  - VM(s) or HW Server(s) that runs pods.
6) Kubelet
7) Kube proxy
8) Container runtime

[video, ru](https://www.youtube.com/watch?v=q_nj340pkQo&list=PLg5SS_4L6LYvN1RqaVesof8KAf-02fJSi)

