# Install tools
```
sudo snap install prometheus
sudo snap install grafana
```

# Configurations
```
/var/snap/<package_name>
```

## For exmaple , change prometheus configuration
```
sudo mv prometheus.yml /var/snap/prometheus/current/prometheus.yml
sudo snap restart prometheus
```