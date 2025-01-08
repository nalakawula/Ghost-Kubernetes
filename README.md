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

Check log

```bash
kubectl -n ghost logs -l app=ghost -f

[2025-01-08 07:48:25] INFO [Recommendations] Updating recommendations metadata
[2025-01-08 08:24:40] INFO Worker for job "mentions-email-report" online
[2025-01-08 08:24:40] INFO Worker for job mentions-email-report sent a message: done
[2025-01-08 08:44:32] INFO "GET /favicon.ico" 200 6ms
[2025-01-08 08:44:32] INFO "GET /" 200 840ms
[2025-01-08 08:44:32] INFO "GET /assets/fonts/inter-roman.woff2?v=df1cdcc4e6" 200 7ms
[2025-01-08 08:44:32] INFO "GET /assets/built/screen.css?v=df1cdcc4e6" 200 12ms
[2025-01-08 08:44:32] INFO "GET /public/cards.min.js?v=df1cdcc4e6" 200 10ms
[2025-01-08 08:44:32] INFO "GET /public/cards.min.css?v=df1cdcc4e6" 200 11ms
[2025-01-08 08:44:32] INFO "GET /assets/built/source.js?v=df1cdcc4e6" 200 13ms
```
