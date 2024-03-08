# Prometheus-Grafana 

Use these scripts to connect your data to prometheus and populate your DeFli Dashboard. Note you **must use your own bucket id** in the last file creation 

Grafana > Prometheus > InfluxDb 

***Install and run node_exporter on the node

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v*/node_exporter-*.*-amd64.tar.gz 
```

```bash
tar xvfz node_exporter-*.*-amd64.tar.gz 
```

```bash
cd node_exporter-*.*-amd64
```

```bash
chmod +x node_exporter
```

```bash
./node_exporter
```
To test the metrics are being populated 

```bash
curl http://localhost:8080/data
```

**Install Prometheus**


```bash
wget https://github.com/prometheus/prometheus/releases/download/v*/prometheus-*.*-amd64.tar.gz
```

```bash
tar xvf prometheus-*.*-amd64.tar.gz
```

```bash
cd prometheus-*.*
```

**Create a prometheus configuration file called "prometheus.yml in the same directory as the Prometheus binary with the following content. Please note you change your "job name" as your bucket ID**

```bash
global:
  scrape_interval: 60s

scrape_configs:
  - job_name: defli
    static_configs:
      - targets: ['localhost:8080']

remote_write:
  - url: '<https://prometheus-prod-13-prod-us-east-0.grafana.net/api/prom/push>'
    basic_auth:
      username: '1463871'
      password: 'glc_eyJvIjoiMTA3MTQxOSIsIm4iOiJzdGFjay04NzU2NzItaG0tcmVhZC1kZWZsaS10b2tlbjEiLCJrIjoiTG9HMjF1dklIcDVTdDZTejE4ODdWYTUzIiwibSI6eyJyIjoicHJvZC11cy1lYXN0LTAifX0=' 
```
    
```bash
./prometheus --config.file=./prometheus.yml
```  
If you don’t want to have to start Prometheus directly from the command line every time you want it to run, you can create a systemd service for it.

