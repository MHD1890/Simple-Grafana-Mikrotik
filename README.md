# Grafana-Mikrotik

![logo](https://repository-images.githubusercontent.com/366494855/c62052b8-17c2-47f2-a3ae-0e397a3ef074)

---

## 🐳 Deploy with docker-compose

### Deploy with bash script

```console
https://github.com/MHD1890/Simple-Grafana-Mikrotik.git
cd ./Grafana-Mikrotik
bash ./run.sh --config
```

```console
  You can also pass some arguments to script to set some these options:

    --config: change the user and password to grafana and specify the mikrotik IP address

    --stop: stop docker containers

    --help
```

For example:

```console
    bash run.sh --config
```

[![asciicast](https://asciinema.org/a/nOhuc7LvI6bRWbg7dcvqFQ4Kc.png)](https://asciinema.org/a/nOhuc7LvI6bRWbg7dcvqFQ4Kc)

### deploy with docker-compose manual

1.Change targets ip (192.168.88.1) into file prometheus/prometheus.yml

2.Run

```console
docker-compose up -d
```

3.Open [localhost:3000](http://localhost:3000)

* Grafana login: `admin`

* Password: `mikrotik`

If you want to change the credentials, then edit the ".env" file

---

## Manual deploy

1.add into prometheus.yml

```yml
  - job_name: Mikrotik
    static_configs:
      - targets:
        - 192.168.88.1  # SNMP device IP.
    metrics_path: /snmp
    params:
      module: [mikrotik]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: localhost:9116  # The SNMP exporter's real hostname:port.
```

2.Configure Prometheus and run /snmp/snmp_exporter

3.Add dashboard <https://grafana.com/grafana/dashboards/14420>

---

### Docker snmp_exporter (deprecated)

[![Docker Pulls](https://img.shields.io/docker/pulls/mashinkopochinko/snmp_exporter_mikrotik?logo=docker)](https://hub.docker.com/repository/docker/mashinkopochinko/snmp_exporter_mikrotik)

> amd64-linux container

```console
sudo docker run -d -p 9116:9116 mashinkopochinko/snmp_exporter_mikrotik:latest
```

---
![img1](/readme/screen.png)
