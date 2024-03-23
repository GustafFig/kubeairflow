# Airflow Kubernetes Study

## Running on a kubernetes cluster

### Install Helm
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
rm get_helm.sh
```

### Install Airflow Helm Chart

Add helm repo
```
helm repo add apache-airflow https://airflow.apache.org
```

Install Airflow
```
helm install airflow apache-airflow/airflow --values values/airflow.yaml --namespace airflow --create-namespace
```

### See install status
```
helm status airflow
```
```
helm show values airflow
```
### Making webservice available
You can use helm to see the next steps
```
helm status airflow
```
like making a port-forwading to service
```
kubectl port-forward svc/airflow-webserver 8080:8080 --namespace airflow
```

### Set GIT-SYNC in Airflow
It is setted at [Airflow Helm Git-Sync oficial page](https://airflow.apache.org/docs/helm-chart/stable/manage-dags-files.html)
```
helm upgrade --install airflow apache-airflow/airflow \
  --set dags.persistence.enabled=true \
  --set dags.gitSync.enabled=true
```

## Setup GitLab to make a git sync 

```
helm repo add gitlab http://charts.gitlab.io/
helm install my-gitlab gitlab/gitlab -f values/gitlab.yaml  --namespace gitlab --create-namespace
```

## Todo
1. Usage of a dynamic webserver secret key detected. We recommend a static webserver secret key instead. See the Helm Chart Production Guide for more details.
2. WARNING: You should set a static webserver secret key. Use `helm show chart airflow` to see it
