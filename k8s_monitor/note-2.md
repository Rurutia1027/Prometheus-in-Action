# Install Helm (on Kubernetes)

## Helm Installation & Configure Prometheus Helm Charts 
- Step-1: Download `get_helm.sh` script 
```shell 
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
```

- Step-2: Modify script's authority 
```shell 
chmod 700 get_helm.sh 
```

- Step-3: Run script to install Helm 
```shell
./get_helm.sh 
Downloading https://get.helm.sh/helm-v3.17.3-linux-amd64.tar.gz
Verifying checksum... Done.
Preparing to install helm into /usr/local/bin
helm installed into /usr/local/bin/helm
```

- Step-4: Configure Prometheus Community Helm Charts 
```shell 
controlplane ~ ✖ helm --help 
The Kubernetes package manager

Common actions for Helm:

- helm search:    search for charts
- helm pull:      download a chart to your local directory to view
- helm install:   upload the chart to Kubernetes
- helm list:      list releases of charts

Environment variables:
...

```shell
controlplane ~ ➜  helm repo add Prometheus-community https://prometheus-community.github.io/helm-charts 
"Prometheus-community" has been added to your repositories

controlplane ~ ➜  helm repo update 
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "Prometheus-community" chart repository
Update Complete. ⎈Happy Helming!⎈
```

- Step-5: Use Helm Install `kube-prometheus-stack` 
```shell 
controlplane ~ ✖ helm install prometheus Prometheus-community/kube-prometheus-stack 
NAME: prometheus
LAST DEPLOYED: Fri Apr 25 12:44:50 2025
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
kube-prometheus-stack has been installed. Check its status by running:
  kubectl --namespace default get pods -l "release=prometheus"

Get Grafana 'admin' user password by running:

  kubectl --namespace default get secrets prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo

Access Grafana local instance:

  export POD_NAME=$(kubectl --namespace default get pod -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=prometheus" -oname)
  kubectl --namespace default port-forward $POD_NAME 3000

Visit https://github.com/prometheus-operator/kube-prometheus for instructions on how to create & configure Alertmanager and Prometheus instances using the Operator.
```

- Get Deploy Info after Install finished 
```
controlplane ~ ➜  kubectl get deploy
NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
prometheus-grafana                    1/1     1            1           86s
prometheus-kube-prometheus-operator   1/1     1            1           86s
prometheus-kube-state-metrics         1/1     1            1           86s
```

There are 3 deployment: 
- `prometheus-grafana` - This is the grafana instance that get deployed with the helm chart. 
- `prometheus-kube-prometheus-operator` - Reponsible for deploying & managing the prometheus instances. 
- `prometheus-kube-state-metrics` - Collects cluster level metrics (pods, deployments, etc). 

- Check how many statefulsets have been deployed by the helm chart ?

```shell 
controlplane ~ ➜  kubectl get statefulsets --namespace default 
NAME                                                   READY   AGE
alertmanager-prometheus-kube-prometheus-alertmanager   1/1     4m14s
prometheus-prometheus-kube-prometheus-prometheus       1/1     4m14s
```

There are 2 statefulsets that have been deployed by the **helm chart**: 
- `alertmanager-prometheus-kube-prometheus-alertmanager`: this is the alertmanager instance. 
- `prometheus-prometheus-kube-promethues-prometheus`: this is the prometheus instance. 


- We continue to check out what type of the **services** Prometheus helm chart created ? 

```shell

controlplane ~ ➜  kubectl get services --namespace default 
NAME                                      TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)                      AGE
alertmanager-operated                     ClusterIP   None           <none>        9093/TCP,9094/TCP,9094/UDP   9m46s
kubernetes                                ClusterIP   10.43.0.1      <none>        443/TCP                      85m
prometheus-grafana                        ClusterIP   10.43.117.17   <none>        80/TCP                       9m52s
prometheus-kube-prometheus-alertmanager   ClusterIP   10.43.33.49    <none>        9093/TCP,8080/TCP            9m52s
prometheus-kube-prometheus-operator       ClusterIP   10.43.3.199    <none>        443/TCP                      9m52s
prometheus-kube-prometheus-prometheus     ClusterIP   10.43.56.7     <none>        9090/TCP,8080/TCP            9m52s
prometheus-kube-state-metrics             ClusterIP   10.43.30.27    <none>        8080/TCP                     9m52s
prometheus-operated                       ClusterIP   None           <none>        9090/TCP                     9m46s
prometheus-prometheus-node-exporter       ClusterIP   10.43.5.113    <none>        9100/TCP                     9m52s
```

- It shows that it is the **ClusterIP** services that prometheus helm chart created. 

The prometheus instance is using a **ClusterIP** service called `prometheus-kube-prometheus-prometheus`, which means it is only accessible within the cluster, and we cannot access it from the external cluster UI side. 

Now we gonna, update the `prometheus-kube-proemtheus-prometheus` service to change it to the **NodePort** service type, also make sure to use node port **30111**. Once **NodePort** is created ok, we should be able to access the Prometheus UI using **Prometheus** button on the top bar. 

```shell 
kubectl edit svc prometheus-kube-prometheus-prometheus
# add - nodePort: 30111, modify type from ClusterIP -> NodePort 
controlplane ~ ➜  kubectl get service --namespace default 
NAME                                      TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)                         AGE
...
prometheus-kube-prometheus-prometheus     NodePort    10.43.56.7     <none>        9090:30111/TCP,8080:30549/TCP   14m
...


controlplane ~ ➜  curl -X GET https://30111-port-zdl5w7jwvovmszkf.labs.kodekloud.com/
<a href="/query">Found</a>.

```

- Now configure an ingress to forward traffic to the prometheus service. here is the ingress.yaml script
```yml 
apiVersion: networking.k8s.io/v1
kind: Ingress 
metadata:
  name: prom-ingress 
  annotations:
spec:
  rules:
    - host: prometheus.kk-demo.com 
      http:
        paths:
          - path: /
            pathType: Prefix 
            backend: 
              service:
                name: prometheus-kube-prometheus-proemtheus
                port:
                  number: 9090
```
- Apply this ingress.yml 

```shell 
controlplane ~ ✖ kubectl apply -f ingress.yaml 
ingress.networking.k8s.io/prom-ingress created
```







## References 
- [Helm Installation Official Documentation](https://helm.sh/docs/intro/install/)