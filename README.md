# mongodb-grafana-monitoring
1. Navigate to #terraform-scripts folder and RUN terraform apply -auto-approve. This command will create a GKE cluster in asia-south1 region with min 3 - max 4 "e2-medium" nodes.
2. Next apply all the files in the #gc-yml-files folder with #kubectl apply -f < respective-file-name > which creates a StorageClass, PersistentVolumeClaims, Statefulset with 3 mongodb replicas and a clusterip service.
3. Install PROMETHEUS, ALERTMANAGER and GRAFANA using HELM charts by modifying few values which I have provided.
  #helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
  #helm repo update
  #helm install prometheus prometheus-community/kube-prometheus-stack -f < custom values file-name >
  #helm install mongodb-exporter prometheus-community/prometheus-mongodb-exporter -f < custom values file-name >
4. After all the PODS of PROMETHEUS, ALERTMANAGER, MONGODB-EXPORTER and GRAFANA and there services are up and running expose there service by following commands:
  #kubectl expose service prometheus-kube-prometheus-prometheus --type=NodePort --target-port=9090 --name=< provide the name which you want to give for exposed prometheus-nodeport-service >
  #kubectl expose service < grafana-service name > --type=NodePort --target-port=3000 --name=< provide the name which you want to give for exposed grafana-nodeport-service >
5. Connect to the prometheus and grafana using nodes-ipaddress:targetport and grafana requries username and password.
     USERNAME = admin
     PASSWORD = prom-operator
7. Once login was successfully completed import the #MongoDB-Grafana-custom-dashboard.json dashboard into grafana.
8. The MongoDB-Grafana-custom-dashboard.json dashboard provides information of:
   1.Node Summary.
     a.RAM.
     b.Disk capacity.
     c.Memory available.
     d.Disk space available.
     e.CPU usge.
   2.Replicaset Metrics.
     a.Uptime.
     b.connections.
     c.Members Health.
     d.Member state.
     e.Replicaset Query Operations.
   3.Query Metrics.
     a.Document Operations.
   4.Resource Metrics.
     a.Network I/O.
     b.Disk I/O.
     c.OPlogSize.
