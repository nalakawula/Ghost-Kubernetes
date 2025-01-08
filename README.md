# Deploy Ghost to Kubernetes

This repository contain manifests to deploy [ghost](https://ghost.org/) to kubernetes,
in my case i use [K3S](https://k3s.io/).

First of all copy `.env.example` to `.env`, then update the value.
After that deploy with following command:

```bash
# create ghost namespace
kubectl create ns ghost

# check generated manifests
kubectl kustomize ./

# deploy
kubectl apply -k ./
```

Then check the result:

```bash
kubectl -n ghost get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/ghost-deployment-599cfd79f-twbs4   1/1     Running   0          11m

NAME                TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
service/ghost-svc   ClusterIP   10.43.96.209   <none>        80/TCP    22h

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/ghost-deployment   1/1     1            1           25h

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/ghost-deployment-599cfd79f    1         1         1       11m
replicaset.apps/ghost-deployment-64bd5485c7   0         0         0       24h
replicaset.apps/ghost-deployment-845d5f856c   0         0         0       25h
replicaset.apps/ghost-deployment-857cd477bd   0         0         0       25h
```
