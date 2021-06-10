#### reference
https://airflow.apache.org/docs/helm-chart/stable/parameters-ref.html
https://airflow.apache.org/docs/helm-chart/stable/index.html




### set repo helm airflow
helm repo add apache-airflow https://airflow.apache.org
helm repo update


helm repo list
NAME          	URL                       
apache-airflow	https://airflow.apache.org

helm search repo airflow
NAME                  	CHART VERSION	APP VERSION	DESCRIPTION                                       
apache-airflow/airflow	1.0.0        	2.0.2      	Helm chart to deploy Apache Airflow, a platform...


### Create namespace
kubectl create namespace airflow


### install airflow
##### Airflow 2.0 allows users to run multiple schedulers. This feature is only recommended for PostgreSQL
helm install airflow apache-airflow/airflow --namespace airflow \
--set airflowVersion=2.0.2 \
--set executor=CeleryExecutor \
--set defaultAirflowTag=2.0.2 \
--set flower.enabled=True \
--set webserver.replicas=2 \
--set scheduler.replicas=2 \
--set pgbouncer.enabled=True \
--set pgbouncer.maxClientConn=200 \
--set pgbouncer.metadataPoolSize=20 \
--set pgbouncer.resultBackendPoolSize=10 \
--set workers.replicas=3 \
--set workers.persistence.enabled=True \
--set workers.terminationGracePeriodSeconds=600 \
--set workers.persistence.size=80Gi




## uninstall airflow
helm delete airflow --namespace airflow





## login airflow ui and flower ui
kubectl port-forward svc/airflow-webserver 8080:8080 --namespace airflow
kubectl port-forward svc/airflow-flower 5555:5555 --namespace airflow

## Dockerfile and DAGS structure
.
├── dags
│   ├── example
│   │   └── example.py
│   └── generated
│       └── example-generated.py
└── Dockerfile







### Deploy new DAG
docker build --no-cache -t zeus-airflow .
docker tag zeus-airflow asia.gcr.io/zeus-cloud/zeus-airflow:0.6
docker push asia.gcr.io/zeus-cloud/zeus-airflow:0.6


helm upgrade --install airflow apache-airflow/airflow --namespace airflow \
--set airflowVersion=2.0.2 \
--set executor=CeleryExecutor \
--set defaultAirflowTag=2.0.2 \
--set flower.enabled=True \
--set webserver.replicas=2 \
--set scheduler.replicas=2 \
--set pgbouncer.enabled=True \
--set pgbouncer.maxClientConn=200 \
--set pgbouncer.metadataPoolSize=20 \
--set pgbouncer.resultBackendPoolSize=10 \
--set workers.replicas=3 \
--set workers.persistence.enabled=True \
--set workers.terminationGracePeriodSeconds=600 \
--set workers.persistence.size=80Gi \
--set images.airflow.repository=asia.gcr.io/zeus-cloud/zeus-airflow \
--set images.airflow.tag=0.6 \
--set images.airflow.pullPolicy=Always




