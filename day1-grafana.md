# Grafana


### Monitoring and Observability


**Monitoring:**

- Tracking known metrics and logs to ensure the system is running as expected.
- Detect problems quickly based on predefined rules.
- e.g., alert if a server CPU crosses 90% usage, check if a service is up or down.


**Observability:**

-  Understand the internal state by analyzing metrics, logs, and traces.
-  The ability to ask any question about your system — even about unknown issues.
-  e.g., multiple users experience network lag


**Telemetry:**

Telemetry is the automatic collection and transmission of data from remote systems to a central location for monitoring and analysis.

- Metrics: Numbers that show how your system is performing. e.g., CPU usage, Memory usage, and request count per second.
- Logs: Time-stamped record of discrete events that happened in your system. Helps track what happened, when, and where. e.g., Error Logs, Access logs, and Application logs.
- Traces: Records of the path of a request through a distributed system, showing the flow and latency across various services. e.g., A trace showing a user login request flowing through different services.


reference: [monitoring and observability](https://www.ibm.com/think/topics/observability-vs-monitoring)


### Prometheus - Monitoring

- An open-source monitoring and alerting toolkit designed for reliability and scalability, with a focus on collecting and storing time-series metrics data.
- Used to collect metrics from configured targets at specified intervals, evaluate rule expressions, display results, and trigger alerts when conditions are met.

[prometheis-overview](https://prometheus.io/docs/introduction/overview/)

### Grafana - Observability

- An open-source analytics and interactive visualization platform that connects to various data sources to create comprehensive dashboards.
- Used to visualize, analyze, and understand metrics from multiple data sources through customizable dashboards, enabling better insights into system behavior.


### Prometheus and Grafana setup on a Linux server:

#### node exporter


```bash

# node exporter

wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz

tar -xvf node_exporter-1.9.1.linux-amd64.tar.gz

cd node_exporter-1.9.1.linux-amd64

./node_exporter & 
# Once the Node Exporter is installed and running, you can verify that metrics are being exported by cURLing the /metrics endpoint


# prometheus
sudo useradd --no-create-home --shell /bin/false prometheus # Running Prometheus under its own restricted user limits the damage if it's ever compromised.
wget https://github.com/prometheus/prometheus/releases/download/v3.4.0-rc.0/prometheus-3.4.0-rc.0.linux-amd64.tar.gz

tar -xvf prometheus-3.4.0-rc.0.linux-amd64.tar.gz
cd rometheus-3.4.0-rc.0.linux-amd64
vim prometheus.yml
# add scrape config to prometheus.yml

./prometheus --config.file=prometheus.yml &

# access prometheus: http://<EC2-IP>:9090

# grafana
sudo apt-get install -y apt-transport-https software-properties-common wget
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
# Updates the list of available packages
sudo apt-get update
# Installs the latest OSS release:
sudo apt-get install grafana
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

**`prometheus.yml`**

```bash
scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']
```

Node exporter full dashboard - 1860

- [node_exporter download](https://prometheus.io/download/#node_exporter)
- [prometheus download](https://prometheus.io/download/)
- [monitoring linux host metrics using node exporter](https://prometheus.io/docs/guides/node-exporter/)
- [grafana installation](https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/)


**Assignment:**

1. Set up the Blackbox Exporter to monitor an HTTP endpoint (e.g., http://example.com) and visualize the results in Grafana using a pre-built dashboard.




