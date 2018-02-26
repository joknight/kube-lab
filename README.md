# Kube-Lab

Lap setup and provisioning of k8s nodes.

Vagrantfile is based on the Kubespray version.

---

`$ kubectl create -f deployments/app-deployment.yaml`

`$ kubectl create -f deployments/kuard-deployment.yaml`

`$ kubectl get pods`
```
NAME                     READY     STATUS    RESTARTS   AGE
app1-5d4d466cc7-8g58g    1/1       Running   0          7m
app1-5d4d466cc7-r549b    1/1       Running   0          7m
app2-55cf97d86d-gz7fd    1/1       Running   0          7m
app2-55cf97d86d-hbh4l    1/1       Running   0          7m
kuard-5dbb87cd56-hnsd6   1/1       Running   0          7m
kuard-5dbb87cd56-scjlb   1/1       Running   0          7m
```

`$ kubectl create -f services/app-service.yaml`

`$ kubectl create -f services/kuard-service.yaml`

`$ kubectl get svc`
```
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
appsvc1      ClusterIP   10.233.14.60    <none>        80/TCP           7m
appsvc2      ClusterIP   10.233.50.77    <none>        80/TCP           7m
kuard        ClusterIP   10.233.49.152   <none>        8080/TCP         7m
kubernetes   ClusterIP   10.233.0.1      <none>        443/TCP          54m
```

`$ kubectl create namespace ingress`

`$ kubectl get namespace`
```
NAME          STATUS    AGE
default       Active    55m
ingress       Active    6s
kube-public   Active    55m
kube-system   Active    55m
```

`$ kubectl create -f deployments/default-backend-deployment.yaml -n=ingress`

`$ kubectl get pods -n ingress`
```
NAME                               READY     STATUS    RESTARTS   AGE
default-backend-5469b58488-9mdkn   1/1       Running   0          39s
default-backend-5469b58488-kz55q   1/1       Running   0          39s
```

`$ kubectl create -f services/default-backend-service.yaml -n=ingress`

`$ kubectl get svc -n ingress`
```
NAME              TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
default-backend   ClusterIP   10.233.12.33   <none>        80/TCP    1m
```

`$ kubectl create -f ingress/nginx-ingress-controller-config-map.yaml -n ingress`

`$ kubectl create -f rbac/nginx-ingress-controller-roles.yaml -n=ingress`

`$ kubectl create -f deployments/nginx-ingress-controller-deployment.yaml -n=ingress`

`$ kubectl get pods -n ingress`
```
NAME                                        READY     STATUS    RESTARTS   AGE
default-backend-5469b58488-9mdkn            1/1       Running   0          6m
default-backend-5469b58488-kz55q            1/1       Running   0          6m
nginx-ingress-controller-6fc7d7fd85-dmp8z   1/1       Running   0          1m
nginx-ingress-controller-6fc7d7fd85-fj6l4   1/1       Running   0          1m
nginx-ingress-controller-6fc7d7fd85-rf4t6   1/1       Running   0          1m
```

`$ kubectl create -f ingress/nginx-ingress.yaml -n=ingress`

`$ kubectl get ingress -n ingress`
```
NAME            HOSTS              ADDRESS   PORTS     AGE
nginx-ingress   test.lab.knic.io             80        11s
```

`$ kubectl create -f ingress/app-ingress.yaml`

`$ kubectl create -f ingress/kuard-ingress.yaml`

`$ kubectl get ingress`
```
NAME            HOSTS               ADDRESS   PORTS     AGE
app-ingress     app.lab.knic.io               80        15s
kuard-ingress   kuard.lab.knic.io             80        2s
```


`$ kubectl create -f services/nginx-ingress-controller-service.yaml -n=ingress`

`$ kubectl get svc -n ingress`
```
NAME              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                        AGE
default-backend   ClusterIP   10.233.12.33    <none>        80/TCP                         9m
nginx-ingress     NodePort    10.233.33.191   <none>        80:30000/TCP,18080:32000/TCP   8s
```


`$ kubectl get all`

```
NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deploy/app1    2         2         2            2           21m
deploy/app2    2         2         2            2           21m
deploy/kuard   2         2         2            2           21m

NAME                  DESIRED   CURRENT   READY     AGE
rs/app1-5d4d466cc7    2         2         2         21m
rs/app2-55cf97d86d    2         2         2         21m
rs/kuard-5dbb87cd56   2         2         2         21m

NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deploy/app1    2         2         2            2           21m
deploy/app2    2         2         2            2           21m
deploy/kuard   2         2         2            2           21m

NAME                  DESIRED   CURRENT   READY     AGE
rs/app1-5d4d466cc7    2         2         2         21m
rs/app2-55cf97d86d    2         2         2         21m
rs/kuard-5dbb87cd56   2         2         2         21m

NAME                        READY     STATUS    RESTARTS   AGE
po/app1-5d4d466cc7-8g58g    1/1       Running   0          21m
po/app1-5d4d466cc7-r549b    1/1       Running   0          21m
po/app2-55cf97d86d-gz7fd    1/1       Running   0          21m
po/app2-55cf97d86d-hbh4l    1/1       Running   0          21m
po/kuard-5dbb87cd56-hnsd6   1/1       Running   0          21m
po/kuard-5dbb87cd56-scjlb   1/1       Running   0          21m

NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
svc/appsvc1      ClusterIP   10.233.14.60    <none>        80/TCP           20m
svc/appsvc2      ClusterIP   10.233.50.77    <none>        80/TCP           20m
svc/kuard        ClusterIP   10.233.49.152   <none>        8080/TCP         20m
svc/kubernetes   ClusterIP   10.233.0.1      <none>        443/TCP          1h
```
