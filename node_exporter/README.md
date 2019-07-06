## Node Exporter 0.16+ for Prometheus monitoring dashboard
#### Tested on Grafana v6.2.5 + node_exporter 0.16, node_exporter 0.17, node_exporter 0.18
Practically, Node Exporter v0.18 was mainly used for streamline and optimize important indicators for display.
Includes: CPU, memory, disk IO, network traffic, temperature, and other monitoring indicators.
##### Screenshot
![](https://github.com/starsliao/Prometheus/raw/master/node_exporter/Node_Exporter.png)
##### 关注公众号【**全栈运维开发 Python & Vue**】 more...
![](https://raw.githubusercontent.com/starsliao/Prometheus/master/qr.png)
#### Caution：
##### Need to install Pie chart plugin：
```
grafana-cli plugins install grafana-piechart-panel
# Please make sure that you can add a pie chart after installation.
```

#### Please configure the variables in grafana panel according to the actual situation of use：

- **Must-do：`$node` takes the value of `instance` from node_exporter, IP+port number format. Most of the panel queries are associated with this variable, please make sure the variable is valid**：
  - Caution：Use the query `count(node_exporter_build_info) by(instance,version)` in Prometheus to check instance format and version of each node.
```
Query with $name：
label_values(node_exporter_build_info{name='$name'},instance)

+If you can't get $name, you can modify into:
label_values(node_exporter_build_info,instance)
```
---
- Important：`$maxmount` is used to query the maximum partition mount points based on the current host of `$node`.
```
query_result(topk(1,sort_desc (max(node_filesystem_size_bytes{instance=~'$node',fstype=~"ext4|xfs"}) by (mountpoint))))
```
---
- Optional：`$env` is custom host environment：
```
label_values(node_exporter_build_info,env)
```
---
- Optional：`$name` is user defined hostname.（It is related with `$env`）：
```
label_values(node_exporter_build_info{env='$env'},name)
```
### 【update】：
##### 2019/7/1
1. Usage graph for disk partitions was added.
2. Optimized data display performance.
3. Usabiliy test on Grafana 6.2.5
##### 2019/5/20
1. Multi-select support in server list was added, and graphs can display data from multiple servers.
2. Optimized display of variables.
3. 优化了部分监控指标的描述说明，点击各图表左上角的“i”即可查看。
##### 2019/1/9
1. Fixed a bug that showed inaccurate memory usage.
2. Additional updates for node_exporter instrument panel and external chain.
3. Tested on Grafana v5.4.2 + node_exporter 0.16 、node_exporter 0.17
##### 11/16
+1. Descriptions of variables were added.
+2. Display speed was optimized after install new panels.
##### 11/15  
1. Environment for each server group was added.
2. Pie chart with total disk space was added.
3. Currently opened file descriptor was added.
4. Descriptions of some monitoring indicators were added.
5. Optimized display results of some indicators.
##### 11/13  
1. Disk I/O operations per second with time-consuming ratio graph were added.
