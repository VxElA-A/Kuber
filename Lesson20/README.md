# Prometheus-K8s-Cluster-Setup-Helm
This repository contains the steps to setup a Prometheus cluster setup using Helm Charts. Easy and straightforward to make it.


# Pre-Requiste
You should have a clean minikube/ docker-desktop kubernetes cluster ready.
Helm Should also be installed. For installation links, check here -> https://helm.sh/docs/intro/install/

# Commands to run 

"helm repo add prometheus-community https://prometheus-community.github.io/helm-charts"

"helm repo update"

"helm install prometheus prometheus-community/kube-prometheus-stack"

### This would have installed all the services and deployments required to have prometheus in your cluster.
To Verify this, just run "kubectl get all "

### We can know more information about the deployment by parsing the details in a yaml file like the below commands:

"kubectl get statefulset"

"kubectl describe statefulset prometheus-prometheus-kube-prometheus-prometheus > prom.yaml"

"kubectl describe deployment prometheus-kube-prometheus-operator> operator.yaml"

# To Access Grafana UI
"kubectl get pod"  -> get the grafana pod name

"kubectl logs prometheus-grafana-84994bb66-qfgrr" -> find port number and user name

"kubectl port-forward deployment/prometheus-grafana 3000" -> do port forwarding to be able to access the grafana UI. WE are using deplopyment name here.

Open browser and wrtie "localhost:3000" to access grafana Ui . Id/Pwd -> admin/prom-operator

# To access prometheus UI

" kubectl get svc "  -> get the service name of prometheus

" kubectl port-forward service/prometheus-kube-prometheus-prometheus 9090 "

We can see that servicemonitors are monitoring all the metrics that Prometheus will get.
"kubectl get servicemonitor"
"kubectl get crd"

To see any deployment/servicemonitor/pod file , use the below command
"kubectl get {name} -oyaml"

# Create MongoDb pods and exporter
copy the mongodb.yaml file and run the below command from same path structure where the file is present

" kubectl apply -f mongodb.yaml "

To monitor application(like mongodb), we will use exporter. Normally, all 3rd party application will have their own exporter. Exporter is basically translator between application metrics and converting them to prometheus format metrics. 
Prometheus will get metrics via the exporter

### More info on exporter

For any application that we want to monitor, we would require 3 component
1- Exporter application ( this will expose the metric endpoint/url)
2- Service ( for connecting to the exporter)
3- ServiceMonitor ( so that the application can be discovered by prometheus. It needs to know that new application is there to monitor)

We can do all this, or just directly run helm chart command for it.

"helm repo add prometheus-community https://prometheus-community.github.io/helm-charts"

"helm repo update"

"helm install mongodb-exporter prometheus-community/prometheus-mongodb-exporter -f value.yaml"

To check if everythign is setup -
"helm show values prometheus-community/prometheus-mongodb-exporter"


*SIDE NOTE - We can override values using chart parameters if we dont want default setting. We can know this by using*

*" helm show values {chart name} "*

# To verify the metrics

"kubectl port-forward service/mongodb-exporter-prometheus-mongodb-exporter 9216"  -> we can see mongoDB metric exposed by exporter "

Can go and check prometheus UI to see the metrics being monitored. (Port forwarding done in above command)
We would also do port forwarding on Grafana to see the charts. (Same as above commands)


