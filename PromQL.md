# Prometheus process
```
# check machine up or down
up{job="prometheus"}

# prometheus memory storage common metrice , can use it to check there are any exceptions of memory
prometheus_tsdb_head_chunks

# same as last  
prometheus_tsdb_head_series

# check prometheus pull metrics from any targets spends time
scrape_duration_seconds
```
# Node level
```
# node memory usage
100 * (1 - ((avg_over_time(node_memory_MemFree_bytes{job="prometheus-node"}[10m]) + avg_over_time(node_memory_Cached_bytes{job="prometheus-node"}[10m]) + avg_over_time(node_memory_Buffers_bytes{job="prometheus-node"}[10m])) / avg_over_time(node_memory_MemTotal_bytes{job="prometheus-node"}[10m])))

# node disk usage
100 - (100 * ((node_filesystem_avail_bytes{job="prometheus-node",mountpoint="/",fstype!="rootfs"} )  / (node_filesystem_size_bytes{job="prometheus-node",mountpoint="/",fstype!="rootfs"}) ))

# node cpu usage
100 - (avg by (job) (irate(node_cpu_seconds_total{job="prometheus-node",mode="idle"}[5m])) * 100)
```

# Container level
```
# cpu usage by container stack name - in swarm mode , notice !! divided by '8' is the amount of machine cpu cores
## this is percentage
sum(irate(container_cpu_usage_seconds_total{job="{YOUR_JOB_NAME}}",container_label_com_docker_swarm_node_id=~'.+'}[1m])) by (container_label_com_docker_stack_namespace) * 100 / 8

# memory usage by container stack name - in swarm mode
## unit is bytes , so you can setting to convert GB or MB 
avg(container_memory_usage_bytes{job="{YOUR_JOB_NAME}}",container_label_com_docker_swarm_node_id=~'.+'}) by (container_label_com_docker_stack_namespace)

# recieve traffic 
sum(irate(container_network_receive_bytes_total{job="uat-cadvisor",container_label_com_docker_swarm_node_id=~'.+'}[5m])) by (container_label_com_docker_stack_namespace)

# transmit traffic
sum(irate(container_network_transmit_bytes_total{job="uat-cadvisor",container_label_com_docker_swarm_node_id=~'.+'}[5m])) by (container_label_com_docker_stack_namespace)
```