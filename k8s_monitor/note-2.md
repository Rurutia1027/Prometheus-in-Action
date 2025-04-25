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
```

- Step-4: Configure Prometheus Community Helm Charts 
```shell 
helm --help 

helm repo add Prometheus-community https://prometheus-community.github.io/helm-charts 

helm repo update 
```

- Step-5: Use Helm Install `kube-prometheus-stack` 
```shell 
helm install prometheus prometheus-community/kube-prometheus-stack   
```



## References 
- [Helm Installation Official Documentation](https://helm.sh/docs/intro/install/)