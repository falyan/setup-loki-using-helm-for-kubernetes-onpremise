# setup loki using helm for kubernetes on-premise
setup loki using helm for k8s on premise
![Alt text](image.png)

# Components <br />
**Loki:** The main server component is called Loki. It is responsible for permanently storing the logs it is being shipped and it executes the LogQL queries from clients. Loki shares its high-level architecture with Cortex, a highly scalable Prometheus backend. <br />
**Promtail:** To ship logs to a central place, an agent is required. Promtail is deployed to every node that should be monitored and sends the logs to Loki. It also does important task of pre-processing the log lines, including attaching labels to them for easier querying. <br />
**Grafana:** The Explore feature of Grafana 6.0+ is the primary place of contact between a human and Loki. It is used for discovering and analyzing logs. <br />

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

## Add repository loki using helm chart
```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm repo list
```
![Alt text](image-3.png)

## Deploy Loki and Promtail to your cluster with default value and custome namespace
1. create namespace loki on your cluster
```bash
kubectl create namespace loki
```
2. deploy loki and promtail to loki namespace
```bash
helm upgrade --install loki --namespace=loki grafana/loki-stack
```
![Alt text](image-4.png)

3. watch pod loki and promtail is creating, wait a view minutes 
```bash
watch kubectl get pod -n loki
```
![Alt text](image-5.png)

all pod will up about 2 minutes
![Alt text](image-6.png)
**note:** <br />
**loki-0 :** The main server component <br />
**loki-promtail-xxx** : Agent for share log to loki main server

## open service loki from your cluster with nodeport
1. check your service first
```bash
kubectl get svc -n loki
```
![Alt text](image-7.png)

type default service is **Cluster-IP**, we will change to **NodePort** 
```bash
kubectl edit svc loki -n loki
```
![Alt text](image-8.png)

kubernetes will assign nodeport automatically, check with command below

```bash
kubectl get svc -n loki
```
![Alt text](image-9.png)

**NodePort** listen on **3100:30910/TCP** 

# access your loki service from Grafana
we assume your grafana already installed so we use the grafana to visualize log <br />
open your grafana and add datasource **loki** <br />

1. login into grafana choose connection <br />
![Alt text](image-10.png) <br />
2. add new connection, search loki <br />
![Alt text](image-11.png) <br />
3. add new datasource <br />
![Alt text](image-12.png) <br />
4. add datasourcename and ip source loki from your cluster <br />
![Alt text](image-13.png) <br />
**Name:** dataource name <br />
**URL:** your ip cluster and NodePort **{{http://ip-cluster:NodePort}}** <br />
5. go to explore to see your log <br />
![Alt text](image-14.png) <br />
6. choose your datasourece <br />
![Alt text](image-15.png) <br />
7. choose your label app <br />
![Alt text](image-16.png) <br />
**note :**
select label : **app** <br />
select value : **your pod name** <br />
8. your log apps below <br />
![Alt text](image-18.png) <br />

# Thankyou :D

## 🔗 About me
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/falyan-zuril-587585247/)













