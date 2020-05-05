---
title: "Using prometheus to monitor docker statistics"
description: ""
category: "DevOP"
tags: [prometheus,docker]
---
{% include JB/setup %}

[Prometheus](https://prometheus.io/docs/introduction/overview/) is an open-source systems monitoring and alerting toolkit. It is a monitoring framework.  
[Grafana](https://grafana.com/docs/grafana/latest/) allows you to query, visualize, alert on and understand your metrics no matter where they are stored. It is a graphing solution i.e a frontend to prometheus.  
Prometheus has many exporters, agents that export statistics from various apps and systems.  
[cadvisor](https://github.com/google/cadvisor) exports container information.  
[node-exporter](https://github.com/prometheus/node_exporter) exports system information.  

node-exporter not designed to be used in [docker](https://github.com/prometheus/node_exporter#using-docker).  
If you want to use it in docker specify volumes, use bind mounts.  
Taken from [vegasbrianc](https://github.com/vegasbrianc/prometheus/blob/master/docker-compose.yml)
```
  node-exporter:
    image: prom/node-exporter
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
    command: 
      - '--path.procfs=/host/proc' 
      - '--path.sysfs=/host/sys'
      - --collector.filesystem.ignored-mount-points
      - "^/(sys|proc|dev|host|etc|rootfs/var/lib/docker/containers|rootfs/var/lib/docker/overlay2|rootfs/run/docker/netns|rootfs/var/lib/docker/aufs)($$|/)"
```

Edit docker-compose from [vegasbrianc](https://github.com/vegasbrianc/prometheus/blob/master/docker-compose.yml)  
Check for conflicting ports  
Copy [prometheus.yml](https://github.com/vegasbrianc/prometheus/blob/master/prometheus/prometheus.yml)  
`sudo docker-compose up -d`

Check everything works  
open browser:  
1. cadvisor http://your-host-ip:8084/containers/
2. prometheus(all state is up) http://your-host-ip:9090/targets 
3. grafana(add datasource, dashboard) http://your-host-ip:3000/

grafana login use passwd defined in docker-compose.yml, change it after login.

grafana setup steps
1. add datasource [1]({{site.url}}/assets/grafana-add-datasource-1.png), [2]({{site.url}}/assets/grafana-add-datasource-2.png), [3]({{site.url}}/assets/grafana-add-datasource-3.png)  
2. add dashboard [1]({{site.url}}/assets/grafana-import-dashboard-1.png), [2]({{site.url}}/assets/grafana-import-dashboard-2.png), [3]({{site.url}}/assets/grafana-import-dashboard-3.png)

Using dashboard from [11074](https://grafana.com/grafana/dashboards/11074)

Final result
![Grafana Dashboard]({{site.url}}/assets/grafana-dashboard-1.png)  

