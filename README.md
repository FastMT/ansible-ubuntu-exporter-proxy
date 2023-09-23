# ansible-ubuntu-exporter-proxy
Ansible role to install Prometheus Proxy on linux server

## Installation

Create requirements.yml file

```
# Include ubuntu-exporter-proxy role
- src: https://github.com/FastMT/ansible-ubuntu-exporter-proxy.git
  name: ubuntu-exporter-proxy
  version: "v1.0.0"
```

Install external module into ~/.ansible/roles folder

```
ansible-galaxy install -r requirements.yml
```

## Usage

playbook.yml:

```
# Deploy nginx to proxy http calls from prometheus to exporters
- role: "ubuntu-exporter-proxy"
  vars:
    # Allow IPs and subnets with Prometheus servers
    proxy_allow:
      - '10.0.0.0/8'
      - '1.2.3.4'
    # Listen IP - optional parameter (default 0.0.0.0)
    ubuntu_exporter_proxy_listen_ip: "0.0.0.0"
    # Listen port - optional parameter (default 8888)
    ubuntu_exporter_proxy_listen_port: 8888
    # Allowed ports for connects via proxy (default 9100 port only)
    ubuntu_exporter_proxy_allowed_ports:
      - 9100
      - 9101 
```

prometheus.yml:

```
# Example of Prometheus scrape config with HTTP proxy
- job_name: my-exporters
  scheme: http
  metrics_path: /metrics
  proxy_url: http://<server_name>:8888
  file_sd_configs:
    - files:
      - './my-exporters/*.yml'
      refresh_interval: 60s
```
