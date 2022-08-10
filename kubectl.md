# how to use kubectl 

### Autocompletion & context dispaly
#### [Kubectl Cheatshet](https://kubernetes.io/ru/docs/reference/kubectl/cheatsheet/)

### gke switch accounts and contexts

### aws use contexs
#### use --profile & aws sts-get-credentials to get token

### 1. set alias for kubectl to k in your .zsh or bash
### 2. set autocomplete for kubectl
### 3. add context display to your shell (to see which cluster you are on) - kubectx
### 4. kubetail/papertrail
### 5. install lens and used shell from there ;-)
### 6. OhmyZSH kubectl extensions https://agrimprasad.com/post/supercharge-kubernetes-setup/
### 7. kubectl proxy to access dashboard & api from your localhost:
![image](https://user-images.githubusercontent.com/17558124/183903966-b01e5d77-f91f-4a1f-91d8-68da762af35b.png)



Q:How to tail logs from all pods in a deployment?
A: use -l (label selector)  and -f for follow!
#kubectl logs -f -l app=example
****
**Now each pod will print out its name along with the timestamp. If we want to watch all those different pods and the logs they’re producing, first use the kubectl log deployment/example. Sadly, this will only pick one of the pods. However, there’s a way to look at all of them. See the following:

> kubectl logs -f -l app=example

example-5bc8d9cddc-bzpjj Tue Sep 1 11:47:03 UTC 2020

example-5bc8d9cddc-qppfk Tue Sep 1 11:47:04 UTC 2020

example-5bc8d9cddc-lnmw4 Tue Sep 1 11:47:04 UTC 2020

example-5bc8d9cddc-bzpjj Tue Sep 1 11:47:04 UTC 2020

example-5bc8d9cddc-qppfk Tue Sep 1 11:47:05 UTC 2020

example-5bc8d9cddc-lnmw4 Tue Sep 1 11:47:05 UTC 2020

example-5bc8d9cddc-bzpjj Tue Sep 1 11:47:05 UTC 2020

example-5bc8d9cddc-qppfk Tue Sep 1 11:47:06 UTC 2020
The command takes a flag (-l / –selector) that lets you filter by label. Since we tag our deployment pods with the example label, we can see all of them. The -f / –follow flag then continuously prints out the message from those pods.

**

