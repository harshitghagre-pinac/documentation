# Installation

## Promethus 
[Promethus Documentation for Installation](https://prometheus.io/download/)

```
wget https://github.com/prometheus/prometheus/releases/download/v3.5.0/prometheus-3.5.0.linux-amd64.tar.gz
tar -xvf prometheus-3.5.0.linux-amd64.tar.gz
rm prometheus-*.tar.gz
sudo mkdir /etc/prometheus /var/lib/prometheus
cd prometheus-3.5.0.linux-amd64
sudo mv prometheus promtool /usr/local/bin/
sudo mv prometheus.yml /etc/prometheus/prometheus.yml
sudo mv consoles/ console_libraries/ /etc/prometheus/
prometheus --version

```
Verify Promethus Config 
```
promtool check config /etc/prometheus/prometheus.yml

```
## Configure Promethus as Service 

```
sudo useradd -rs /bin/false prometheus
sudo chown -R prometheus: /etc/prometheus /var/lib/prometheus
sudo nano /etc/systemd/system/prometheus.service

[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries \
    --web.listen-address=0.0.0.0:9090 \
    --web.enable-lifecycle \
    --log.level=info

[Install]
WantedBy=multi-user.target

sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus
sudo systemctl status prometheus

```

## Node Exporter 
[NodeExporter Documentation for Installation](https://prometheus.io/download/)

```
wget https://github.com/prometheus/node_exporter/releases/download/v1.10.2/node_exporter-1.10.2.linux-amd64.tar.gz
tar xvfz node_exporter-1.10.2.linux-amd64.tar.gz
sudo mv node_exporter-1.10.2.linux-amd64/node_exporter /usr/local/bin
rm -r node_exporter-1.10.2.linux-amd64*
node_exporter
sudo useradd -rs /bin/false node_exporter
sudo nano /etc/systemd/system/node_exporter.service

[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target

sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl status node_exporter

```

## Configure Promethus to Monitor Client Node

```
sudo nano /etc/prometheus/prometheus.yml
...
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "node"
    static_configs:
      - targets: ["localhost:9100"]

sudo systemctl restart prometheus

```

## Install Grafana and Deploy 

```
sudo apt-get install -y apt-transport-https software-properties-common
sudo wget -q -O /usr/share/keyrings/grafana.key https://apt.grafana.com/gpg.key
echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get update
sudo apt-get install grafana
sudo systemctl daemon-reload
sudo systemctl enable grafana-server.service
sudo systemctl start grafana-server
sudo systemctl status grafana-server

```

## How to integrate Grafana and Promethus

* Open grafana which is on the `port:3000` and log in using username `admin` & default passowrd `password`.
* Configure data source -> add data source -> choose prometheus -> set url `http://localhost:9090` save & test. 


 ## Node_exporter installation

 ```



 ```
# Apache Installation 
```
sudo apt update
sudo apt install -y apache2
sudo systemctl enable apache2
sudo systemctl start apache2
```
## Verfiy Installation 

```
systemctl status apache2
curl http://localhost

```

## Apache Exporter 

```
sudo a2enmod status
sudo nano /etc/apache2/conf-available/status.conf

Add this `<Location "/server-status">
    SetHandler server-status
    Require all granted
</Location>

sudo a2enconf status
sudo systemctl reload apache2

curl http://localhost/server-status?autofdfafdfjdjjjj

wget https://github.com/Lusitaniae/apache_exporter/releases/latest/download/apache_exporter-0.11.0.linux-amd64.tar.gz
tar xvf apache_exporter-*.tar.gz
sudo mv apache_exporter-*/apache_exporter /usr/local/bin/

wget https://github.com/Lusitaniae/apache_exporter/releases/latest/download/apache_exporter-0.11.0.linux-amd64.tar.gz
tar xvf apache_exporter-*.tar.gz
sudo mv apache_exporter-*/apache_exporter /usr/local/bin/

sudo nano /etc/systemd/system/apache_exporter.service
[Unit]
Description=Apache Exporter
After=network.target

[Service]
ExecStart=/usr/local/bin/apache_exporter \
  --scrape_uri=http://localhost/server-status?auto
Restart=always

[Install]
WantedBy=multi-user.target

sudo systemctl daemon-reload
sudo systemctl enable apache_exporter
sudo systemctl start apache_exporter

curl http://localhost:9117/metrics

```
# Install MySQL

```
sudo apt install -y mysql-server
sudo systemctl enable mysql
sudo systemctl start mysql

```
# Verify Installation 

```
systemctl status mysql
mysql -e "SELECT 1;"


```