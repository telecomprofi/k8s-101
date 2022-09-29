## Troubleshoting Out of memory killings of container processes inside pod on kubernetes worker node

### 1. Check CPU/Mem average load on the node with top
### 2. Identify process(es) that consume more memory than others, check dmesg for errors (like oom - 
```
dmnesg -T | grep oom
```
and zombie processes
```
ps -elf | grep Z
```
### 3. in oom messages from dmesg notice messagen consisting of two hashes:  
'oom_memcg=/kubepods/burstable/pod5159b67a-e600-46f3-97d4-ead2586a7b2a/37a6a61a21ca760b00e6886381a7e2e5c88fad687dde884bd4103f830dae0767,task_memcg=/kubepods/burstable/pod5159b67a-e600-46f3-97d4-ead2586a7b2a/37a6a61a21ca760b00e6886381a7e2e5c88fad687dde884bd4103f830dae0767," - it has following format pod<pod-id-meta>/<container-id-meta>
```
[20409.613189] php-fpm invoked oom-killer: gfp_mask=0xcc0(GFP_KERNEL), order=0, oom_score_adj=934
[20409.627732] oom_reaper: reaped process 32055 (php-fpm), now anon-rss:0kB, file-rss:0kB, shmem-rss:19292kB
[20409.631404]  oom_kill_process+0xd7/0x110
[20410.098109] [  pid  ]   uid  tgid total_vm      rss pgtables_bytes swapents oom_score_adj name
[20412.213083] oom-kill:constraint=CONSTRAINT_MEMCG,nodemask=(null),cpuset=37a6a61a21ca760b00e6886381a7e2e5c88fad687dde884bd4103f830dae0767,mems_allowed=0,oom_memcg=/kubepods/burstable/pod5159b67a-e600-46f3-97d4-ead2586a7b2a/37a6a61a21ca760b00e6886381a7e2e5c88fad687dde884bd4103f830dae0767,task_memcg=/kubepods/burstable/pod5159b67a-e600-46f3-97d4-ead2586a7b2a/37a6a61a21ca760b00e6886381a7e2e5c88fad687dde884bd4103f830dae0767,task=php-fpm,pid=29454,uid=1000
[20412.252007] Memory cgroup out of memory: Killed process 29454 (php-fpm) total-vm:409816kB, anon-rss:31200kB, file-rss:13100kB, shmem-rss:19296kB, UID:1000 pgtables:276kB oom_score_adj:934
[20412.272029] oom_reaper: reaped process 29454 (php-fpm), now anon-rss:0kB, file-rss:0kB, shmem-rss:19296kB
[20412.272559] php-fpm invoked oom-killer: gfp_mask=0xcc0(GFP_KERNEL), order=0, oom_score_adj=934
[20412.316849]  oom_kill_process+0xd7/0x110
```
### 4. on kubectl filter for the first hash (after podxxxx, before /) - it will give you info in which pod the prorblematic process runs as  container:
```
kubectl get pod --all-namespaces -o json | jq '.items[] | select(.metadata.uid == "5159b67a-e600-46f3-97d4-ead2586a7b2a")' 
```

### 5. For pod indentified in step 4 filter for specific container inside the pod (second piece of hash from dmesg oom message):
```
kubectl get pod my-pod-6f47444666-4nmbr -o json | jq '.status.containerStatuses[] | select(.containerID == "docker://37a6a61a21ca760b00e6886381a7e2e5c88fad687dde884bd4103f830dae0767")'
```
that will point you to specific container image that causes Out of Memory killings.

#### Notify developer about the issue with their container image.
----------------------------------------------------------------------


Source:
from stackoverflow:


How to do I map this ID to the pod name I see when I run kubectl get pods?

Given the following, /kubepods/burstable/pod99b2fe2a-104d-11e8-baa7-06145aa73a4c, the last bit is the pod UID, and can be resolved to a pod by looking at the metadata.uid property on the pod manifest:

kubectl get pod --all-namespaces -o json | jq '.items[] | select(.metadata.uid == "99b2fe2a-104d-11e8-baa7-06145aa73a4c")'
Once you've resolved the UID to a pod, we can resolve the second UID (container ID) to a container by matching it with the .status.containerStatuses[].containerID in the pod manifest:

~$ kubectl get pod my-pod-6f47444666-4nmbr -o json | jq '.status.containerStatuses[] | select(.containerID == "docker://5339636e84de619d65e1f1bd278c5007904e4993bc3972df8628668be6a1f2d6")'
Furthermore, there are several subpaths in /kubepods, such as /kubepods/burstable and /kubepods/besteffort. What do these mean and how does a given pod fall into one or another of these subpaths?

Burstable, BestEffort, and Guaranteed are Quality of Service (QoS) classes that Kubernetes assigns to pods based on the memory and cpu allocations in the pod spec. More information on QoS classes can be found here https://kubernetes.io/docs/tasks/configure-pod-container/quality-service-pod/.

To quote:

For a Pod to be given a QoS class of Guaranteed:

Every Container in the Pod must have a memory limit and a memory request, and they must be the same.

Every Container in the Pod must have a cpu limit and a cpu request, and they must be the same.

A Pod is given a QoS class of Burstable if:

The Pod does not meet the criteria for QoS class Guaranteed.

At least one Container in the Pod has a memory or cpu request.

For a Pod to be given a QoS class of BestEffort, the Containers in the Pod must not have any memory or cpu limits or requests.

Lastly, where can I learn more about what manages /kubepods?

/kubepods/burstable, /kubepods/besteffort, and /kubepods/guaranteed are all a part of the cgroups hierarchy, which is located in /sys/fs/cgroup directory. Cgroups is what manages resource usage for container processes such as CPU, memory, disk I/O, and network. Each resource has its own place in the cgroup hierarchy filesystem, and in each resource sub-directory are /kubepods subdirectories. More info on cgroups and Docker containers here: https://docs.docker.com/config/containers/runmetrics/#control-groups
