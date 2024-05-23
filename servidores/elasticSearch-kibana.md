---
title: Installation
description: 
published: true
date: 2024-05-23T23:11:42.071Z
tags: 
editor: markdown
dateCreated: 2024-05-23T23:11:42.071Z
---

# Elastic - Kibana
```
#!/bin/bash

# Variables
ES_VERSION="7.8.1"
ES_HOME="/usr/local/elasticsearch"
KIBANA_HOME="/usr/local/kibana"
ES_USER="elastic"
ES_PASSWORD="SUPER-PASSWORD"
JAVA_PACKAGE="openjdk-11-jdk"

# Instalar Java
echo "Instalando Java..."
sudo apt update
sudo apt install -y $JAVA_PACKAGE

# Descargar y configurar Elasticsearch
echo "Descargando Elasticsearch..."
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-$ES_VERSION-linux-x86_64.tar.gz
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-$ES_VERSION-linux-x86_64.tar.gz.sha512
shasum -a 512 -c elasticsearch-$ES_VERSION-linux-x86_64.tar.gz.sha512

echo "Extrayendo Elasticsearch..."
tar -xzf elasticsearch-$ES_VERSION-linux-x86_64.tar.gz
sudo mv elasticsearch-$ES_VERSION $ES_HOME

# Crear usuario para Elasticsearch
sudo adduser --system --no-create-home --group elasticsearch
sudo chown -R elasticsearch:elasticsearch $ES_HOME

# Configurar Elasticsearch
echo "Configurando Elasticsearch..."
sudo tee $ES_HOME/config/elasticsearch.yml > /dev/null <<EOL
cluster.name: "my-application"
node.name: "node-1"
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
network.host: 0.0.0.0
discovery.type: single-node
xpack.security.enabled: true
EOL

# Crear archivo de servicio de Elasticsearch
echo "Creando archivo de servicio para Elasticsearch..."
sudo tee /etc/systemd/system/elasticsearch.service > /dev/null <<EOL
[Unit]
Description=Elasticsearch
Documentation=https://www.elastic.co
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=elasticsearch
Group=elasticsearch
ExecStart=$ES_HOME/bin/elasticsearch
Restart=on-failure
RestartSec=10
LimitNOFILE=65536
LimitMEMLOCK=infinity
Environment=ES_HOME=$ES_HOME
Environment=ES_PATH_CONF=$ES_HOME/config
Environment=PID_DIR=/var/run/elasticsearch
Environment=ES_SD_NOTIFY=true
WorkingDirectory=$ES_HOME

[Install]
WantedBy=multi-user.target
EOL

# Ajustar parámetros del sistema
echo "Ajustando parámetros del sistema..."
sudo sysctl -w vm.max_map_count=262144
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf

# Iniciar y habilitar Elasticsearch
echo "Iniciando y habilitando Elasticsearch..."
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service
sudo systemctl start elasticsearch.service

# Esperar a que Elasticsearch esté completamente iniciado
sleep 20

# Configurar la contraseña del usuario elastic
echo "Configurando la contraseña del usuario elastic..."
curl -u elastic:changeme -X POST "http://localhost:9200/_security/user/elastic/_password" -H "Content-Type: application/json" -d"
{
  \"password\" : \"$ES_PASSWORD\"
}"

# Descargar y configurar Kibana
echo "Descargando Kibana..."
wget https://artifacts.elastic.co/downloads/kibana/kibana-$ES_VERSION-linux-x86_64.tar.gz
tar -xzf kibana-$ES_VERSION-linux-x86_64.tar.gz
sudo mv kibana-$ES_VERSION $KIBANA_HOME

# Crear usuario para Kibana
sudo adduser --system --no-create-home --group kibana
sudo chown -R kibana:kibana $KIBANA_HOME

# Configurar Kibana
echo "Configurando Kibana..."
sudo tee $KIBANA_HOME/config/kibana.yml > /dev/null <<EOL
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]
elasticsearch.username: "elastic"
elasticsearch.password: "$ES_PASSWORD"
EOL

# Crear archivo de servicio de Kibana
echo "Creando archivo de servicio para Kibana..."
sudo tee /etc/systemd/system/kibana.service > /dev/null <<EOL
[Unit]
Description=Kibana
Documentation=https://www.elastic.co
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=kibana
Group=kibana
ExecStart=$KIBANA_HOME/bin/kibana
Restart=on-failure
RestartSec=10
LimitNOFILE=65536
LimitMEMLOCK=infinity
Environment=KBN_PATH_CONF=$KIBANA_HOME/config
WorkingDirectory=$KIBANA_HOME

[Install]
WantedBy=multi-user.target
EOL

# Iniciar y habilitar Kibana
echo "Iniciando y habilitando Kibana..."
sudo systemctl daemon-reload
sudo systemctl enable kibana.service
sudo systemctl start kibana.service

echo "Instalación y configuración de Elasticsearch y Kibana completada."

```

