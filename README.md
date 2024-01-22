# setup loki using helm for kubernetes onpremise
setup loki using helm for k8s on premise
![Alt text](image.png)

## Install Helm on your cluster
1. download binary helm
```bash 
mkdir helm
cd helm
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
ls
```
![Alt text](image-1.png)
2. install binary helm using /bin/bash
```bash
chmod 700 get_helm.sh
./get_helm.sh
helm version
```
![Alt text](image-2.png)




