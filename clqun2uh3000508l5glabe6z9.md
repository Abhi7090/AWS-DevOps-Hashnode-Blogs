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


#Loki and Promtail Installation
#install Loki packages
wget
https://raw.githubusercontent.com/grafana/loki/v2.8.0/cmd/loki/loki-localconfig.yaml -O loki-config.yaml
docker run -d --name loki -v /path/to/loki/config:/mnt/config -p 3100:3100 grafana/loki:2.8.0 --config.file=/mnt/config/loki-config.yaml
#install Promtail packages
wget
https://raw.githubusercontent.com/grafana/loki/v2.8.0/clients/cmd/promtail
/promtail-docker-config.yaml -O promtail-config.yaml
docker run -d --name promtail -v /path/to/loki/config:/mnt/config -v /var/log:/var/log grafana/promtail:2.8.0 --config.file=/mnt/config/promtail-config.yaml
#Check docker logs
docker logs loki
docker logs promtail
```

Add Port numbers in EC2 instance (Security groups)

Port number : 3000 (Grafana)

Port number : 80 (HTTP)

Port number : 3100 (Loki)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704097081251/de19aa9b-1692-4612-b1e2-6ffbf84887bb.jpeg align="center")