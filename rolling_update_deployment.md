## Default rolling update scenario

### Option 1. Stateless app, only container image was updated. No need to change configmaps, volumes, # of replicas:
kubectl set image (do not forget to populate change cause - it wiil help you to roll back (rollout undo) to correct revision
```
kubectl set image deployment.apps/demo-app image=73570586743739.dkr.ecr.us-west-2.amazonaws.com/demo-app:v3.0 kubectl annotate deployment.apps/demo-app kubernetes.io/change-cause="demo version changed from 2.0 to 3.0"

kubectl rollout status deployment.apps/demo-app
```
stream (aka tail -f or monitor) log output from all new canary containers into console
```
kubectl logs -f -l name_canary=demo-app-canary --all-containers --max-log-requests=11 (# the last piece is to increase from default 5) | grep -i error
```

And if deployment fails (pods stuck, restarts etc) roll deployment back to previous 'stable' revision:
```
kubectl rollout undo deployment.apps/demo-app --to-revision=2
kubectl rollout status deployment.apps/demo-app
```

that's it folks :-)

### Option 2. Statefull app - need to think about node affinity when deploying new pods - to make sure pod will be spinned up on the same node.
Statefull set update <tba>
  
 Option 3. Canary Deployment to production
  pre-requisites:
  HPA has to be disabled first and then re-enabled when new canary is stable
  If you need gradual traffic round-robin, you might need to increase number of 'stable replicas' in 'stable' deployment (think 1/10 of traffic will go to each pod if you have 10 replicas)
  create new deployemnt yaml with the 
  - set deployment.name to <existing-deployment-name>-canary
  - same label as stable deployment  ex. name=demo-app
  - add additional label name_canary=<existing-deployment-name>-canary for grouping purposes (kubectl get pods -l name_canary=demo-canary)
  - new image url  (tested locally and in dev, staging env)
  - number of replicas initially small (like 10% of total pod number)
  
```
kubectl apply -f canary_deployment.yaml
```
  
  watch dashboard for CPU, Memory, requests count, errors 4xx, 5xx, 3xx, 2xx
  if everyting ok - scale up gradually canary deployment and scale down 'stable' deployment:
  
```
kubectl scale deployemnt demo-app-canry --relicas=2 (3, 4, 5, up to maximum desired)
kubectl scale deployement demo-app --replicase=9 (8, 7, 5, 2, 1, 0 up to minimum)
```
  
  
  

