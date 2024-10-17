# Blackbox Exporter Installation

❖ Prometheus Blackbox Exporter is a vital tool for any organization that monitors 
external services such as HTTP, DNS, TCP, ICMP, and more

• Step 1: Installing Blackbox Exporter using Helm Chart :
helm repo list -n monitoring
helm repo update -n monitoring
helm install prometheus-blackbox-exporter prometheuscommunity/prometheus-blackbox-exporter -n monitoring

• Step 2: 
After Installation, we will find a new pod and service like below :
Pod Name: prometheus-blackbox-exporter-55c969f74f-9k5nd
Service Name: prometheus-blackbox-exporter

• Step 3: Update the Prometheus Configuration file
1.we are using a custom [ values.yml ] file for Prometheus configurations
2.File Location: /home/optit/
3.Add following lines in [ values.yml ] file to get the scrap _config targets from 
Blackbox exporter to Prometheus 

# Add Blackbox Exporter scrape config
 additionalScrapeConfigs:
 - job_name: "blackbox_exporter"
 metrics path: /probe
 params:
 module: [http_2xx]
 static_configs:
 - targets:
 - https://optit.in/devops-as-a-service/
 - www.parkquility.com
 - http://10.10.30.93:9208/
 - http://10.10.30.93:9205/
 - http://10.10.30.93:9695/
 relabel_configs:
 - source_labels: [__address__]
 target_label: __param_target
 - source_labels: [__param_target]
 target_label: instance
 - target_label: __address__
 replacement: prometheus-blackbox-exporter:9115 
# Add Blackbox service name along with port number 9115 at replacement: in 
above lines
Step 4: add custom endpoints URLs under targets: in above lines 
ex:
- https://optit.in/devops-as-a-service/
 - www.parkquility.com
 - http://10.10.30.93:9208/
 - http://10.10.30.93:9205/
 - http://10.10.30.93:9695/
Step 5: 
Upgrade the Prometheus deployment with new configurations
helm upgrade prometheus prometheus-community/prometheus -n monitoring 
-f values.yml
Step 6:
In Grafana, import a public dashboard with the ID 7587 or use a custom 
dashboard for the Blackbox exporter. There, you will be able to view the status 
of your end URLs 
