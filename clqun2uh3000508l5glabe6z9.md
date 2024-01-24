---
title: "Grafana, Loki, Promtail and Prometheus"
datePublished: Mon Jan 01 2024 08:08:56 GMT+0000 (Coordinated Universal Time)
cuid: clqun2uh3000508l5glabe6z9
slug: grafana-loki-promtail-and-prometheus

---

1. **Grafana:** Grafana is an open-source analytics and monitoring platform. It allows you to query, visualize, and understand your metrics no matter where they are stored. It supports various data sources, including Prometheus, which brings us to the next point.
    
2. **Prometheus:** Prometheus is an open-source monitoring and alerting toolkit designed for reliability and scalability. It's particularly well-suited for dynamic environments like cloud-native applications. Prometheus collects and stores time-series data and provides a powerful query language for analysis.
    
3. **Loki:** Loki is another open-source project, but it focuses on log aggregation. It's designed to be highly efficient and cost-effective for collecting, displaying, and exploring logs. Loki is often used alongside Prometheus to provide a comprehensive observability solution.
    
4. **Promtail:** Promtail is an agent that works with Loki. It's responsible for scraping logs from various sources, such as log files or the systemd journal, and sending them to Loki for storage and indexing. Essentially, Promtail is the bridge between your logs and Loki.
    

In a nutshell, this trio—Grafana, Loki, and Promtail—works together to provide a robust monitoring and logging solution for your applications and infrastructure. You get real-time metrics visualization with Grafana, efficient log aggregation with Loki, and a dedicated agent (Promtail) to collect and forward logs to Loki.

Installation of Grafana, Loki and Promtail

Create a folder name grafana-dashboard  
mkdir grafana-dashboard

```bash
mkdir grafana-dashboard
cd grafana-dashboard
sudo apt-get install -y apt-transport-https
sudo apt-get install -y apt-transport-https software-properties-common wget
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get update
sudo apt-get install grafana
sudo /bin/systemctl status grafana-server
sudo systemctl start grafana-server
sudo systemctl enable grafana-server 
#install docker 
sudo apt update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ubuntu
docker network create loki
#Loki and Promtail Installation
#install Loki packages
wget https://raw.githubusercontent.com/grafana/loki/v2.8.0/cmd/loki/loki-local-config.yaml -O loki-config.yaml
docker run -d --name loki --net loki -p 3100:3100 grafana/loki
#install Promtail packages
wget https://raw.githubusercontent.com/grafana/loki/v2.8.0/clients/cmd/promtail/promtail-docker-config.yaml -O promtail-config.yaml
docker run -d --name promtail --net loki -v $(pwd):/mnt/config -v /var/log:/var/log --link loki grafana/promtail:2.8.0 -config.file=/mnt/config/promtail-config.yaml
#Check docker logs
docker logs loki
docker logs promtail
```

Add Port numbers in EC2 instance (Security groups)

Port number : 3000 (Grafana)

Port number : 80 (HTTP)

Port number : 3100 (Loki)

http://publicIP:3000

username : admin

password : admin

publicIP:3100/ready

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704097081251/de19aa9b-1692-4612-b1e2-6ffbf84887bb.jpeg align="center")

### **Prometheus :**

Prometheus is an open-source monitoring and alerting toolkit designed for reliability and scalability. Here's an overview of Prometheus and a basic installation guide.

### **Prometheus Overview:**

* **Data Model:** Prometheus follows a multi-dimensional data model with time series data identified by metric name and key-value pairs.
    
* **Query Language:** It has a powerful query language called PromQL, allowing for flexible analysis and visualization of collected metrics.
    
* **Alerting:** Prometheus supports alerting based on defined rules, sending notifications when certain conditions are met.
    
* **Scalability:** Prometheus is designed to be highly scalable and can be federated to aggregate data from multiple instances.
    
    * ### **Prometheus Installation:**
        
        1. **Download and Extract:**
            
            ```dockerfile
            Wget
            https://raw.githubusercontent.com/prometheus/prometheus/main/documentation/examples/prom
            etheus.yml
            docker run -d --name=prometheus -p 9090:9090 -v
            <PATH_TO_prometheus.yml_FILE>:/etc/prometheus/prometheus.yml
            prom/prometheus --config.file=/etc/prometheus/prometheus.yml
            #run the port number 
            Grafana at http://localhost:3000
            Prometheus at http://localhost:9090
            Loki at http://localhost:3100
            cAdvisor at http://localhost:8080
            ```
            
            ## **cAdvisor** :
            
        
        Container Advisor is an open-source tool that provides container-level performance monitoring and analysis. It is part of the Google Container Tools project and is designed to give you insights into the resource usage and performance characteristics of running containers.
        
    * Here's a brief overview and a simple installation guide for cAdvisor:
        
        ### **cAdvisor Overview:**
        
        * **Container Metrics:** cAdvisor collects and exports metrics related to CPU usage, memory usage, file system usage, network statistics, and more for each running container.
            
        * **Web UI:** cAdvisor includes a web-based user interface that allows you to visualize container metrics and inspect performance data.
            
        * **Integration with Prometheus:** cAdvisor is often used in conjunction with Prometheus for scalable and long-term storage of container metrics.
            
        * ### **cAdvisor Installation:**
            
            1. **Run cAdvisor Container:**
                
                ```dockerfile
                docker run -d --name=cadvisor -p 8080:8080 --volume=/var/run/docker.sock:/var/run/docker.sock google/cadvisor:latest
                ```
                
                This command starts the cAdvisor container in detached mode, names it "cadvisor," maps port 8080 on the host to port 8080 in the container, and mounts the Docker socket to allow cAdvisor to collect container metrics.
                
            2. **Access cAdvisor Web UI:** Open your browser and go to [`http://localhost:8080`](http://localhost:8080). You should see the cAdvisor web interface.
                
            3. **View Container Metrics:**
                
                * Click on the container names listed to view detailed metrics for each container.
                    
                * Explore different tabs to view CPU usage, memory usage, file system statistics, and more.
                    
            
            ### **Integration with Prometheus:**
            
            If you are using Prometheus for monitoring, you can also configure cAdvisor to expose metrics in Prometheus format. Here's an example:
            
            ```dockerfile
            docker run -d --name=cadvisor -p 8080:8080 --volume=/var/run/docker.sock:/var/run/docker.sock google/cadvisor:latest -prometheus_endpoint="/metrics"
            ```
            
            Now, you can configure Prometheus to scrape cAdvisor metrics by adding it as a target in your Prometheus configuration.
            
            ````dockerfile
            #create cadvisor.yml file
            ```
            scrape_configs:
            - job_name: cadvisor
              scrape_interval: 5s
              static_configs:
              - targets:
                - cadvisor:8080
            ````
            
            ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704119935261/2d2a1704-af64-40b1-8ef9-516d087d34e4.png align="center")
            
            ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704120057926/352cff58-eb2f-4bb5-b13a-22fc021961a9.png align="center")
            
            ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704120084299/56c3f168-90a7-43bf-8032-841f51a1f89a.png align="center")