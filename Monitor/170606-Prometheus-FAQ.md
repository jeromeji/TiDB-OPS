---
title: Prometheus FAQ
date: 2017-06-01 11:40:52
updated: 2017-06-02 21:40:52
categories:
  - Monitor
tags:
  - Prometheus
  - Grafana
  - TiDB
---
# Prometheus FAQ

## Prometheus 升级

- Prometheus 升级
  - Prometheus 组件由 golang 编写, 直接替换 binary 重启服务即可

- TiDB 曾用版本
  - 1.6
  - 1.8
    - 1.8 与 2.0 数据不兼容
  - 2.0
    - https://prometheus.io/docs/prometheus/latest/querying/api/#tsdb-admin-apis
    - alert.rule 格式为 yml

### Prometheus 监控数据异常

- Prometheus 监控数据异常
  - 数据短暂断崖式下降, 日志中打印 `Storage has entered rushed mode.` 信息
  - 相关 issue
    - https://github.com/prometheus/prometheus/issues/2542
    - https://github.com/coreos/prometheus-operator/issues/304
    - https://github.com/prometheus/prometheus/issues/2936
    - https://github.com/prometheus/prometheus/issues/2222

  ```LOG
  time="2017-10-24T10:08:30+08:00" level=warning msg="Storage has entered rushed mode." chunksToPersist=107071 maxChunksToPersist=524288 maxMemoryChunks=10485
  76 memoryChunks=1139421 source="storage.go:1660" urgencyScore=0.8663654327392578

  time="2017-10-24T10:05:39+08:00" level=info msg="Storage has left rushed mode." chunksToPersist=108398 maxChunksToPersist=524288 maxMemoryChunks=1048576 mem
  oryChunks=1121866 source="storage.go:1647" urgencyScore=0.6989479064941406

  time="2017-10-24T10:05:45+08:00" level=warning msg="Storage has entered rushed mode." chunksToPersist=112409 maxChunksToPersist=524288 maxMemoryChunks=10485
  76 memoryChunks=1268143 source="storage.go:1660" urgencyScore=1
  ```

- 额
  - http://www.jianshu.com/p/36f72490a2a0

- 内存问题?

  ```LOG
  time="2017-08-08T15:38:30+08:00" level=info msg="Starting prometheus (version=1.5.2, branch=master, revision=bd1182d29f462c39544f94cc822830e1c64cf55b)" source="main.go:75"
  time="2017-08-08T15:38:30+08:00" level=info msg="Build context (go=go1.7.5, user=root@a8af9200f95d, date=20170210-14:41:22)" source="main.go:76"
  time="2017-08-08T15:38:30+08:00" level=info msg="Loading configuration file /tidb-data/tidb/deploy/conf/prometheus.yml" source="main.go:248"
  time="2017-08-08T15:38:30+08:00" level=error msg="Error opening memory series storage: found existing files in storage path that do not look like storage files compatible with this version of Prometheus; please delete the files in the storage path or choose a different storage path" source="main.go:182"
  ```

#### 启动速度慢

- Prometheus 启动后，发现网页无法打开，查看日志后，发现 Prometheus 启动后一直在 GC
  - Prometheus 2.0 与 1.0.8 底层存储不一致，2.0 更换为了 RocksDB KV 存储
  - 或者使用 kill -SIGHUP PID 用来重新加载配置文件

  ```bahs
  [tidb@Jeff scripts]$ ./run_prometheus.sh
  level=info ts=2018-04-17T03:04:31.495865947Z caller=main.go:220 msg="Starting Prometheus" version="(version=2.2.0, branch=HEAD, revision=f63e7db4cbdb616337ca877b306b9b96f7f4e381)"
  level=info ts=2018-04-17T03:04:31.495932795Z caller=main.go:221 build_context="(go=go1.10, user=root@52af9f66ce71, date=20180308-16:40:42)"
  level=info ts=2018-04-17T03:04:31.495952002Z caller=main.go:222 host_details="(Linux 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 Jeff (none))"
  level=info ts=2018-04-17T03:04:31.495966987Z caller=main.go:223 fd_limits="(soft=1000000, hard=1000000)"
  level=info ts=2018-04-17T03:04:31.498497082Z caller=main.go:504 msg="Starting TSDB ..."
  level=info ts=2018-04-17T03:04:31.498536225Z caller=web.go:382 component=web msg="Start listening for connections" address=:9090
  level=info ts=2018-04-17T03:05:52.153499505Z caller=main.go:514 msg="TSDB started"
  level=info ts=2018-04-17T03:05:52.153580197Z caller=main.go:588 msg="Loading configuration file" filename=/data1/deploy/conf/prometheus.yml
  level=info ts=2018-04-17T03:05:52.177873304Z caller=main.go:491 msg="Server is ready to receive web requests."
  level=info ts=2018-04-17T03:05:57.320867461Z caller=compact.go:394 component=tsdb msg="compact blocks" count=1 mint=1523923200000 maxt=1523930400000
  level=info ts=2018-04-17T03:06:03.658563096Z caller=head.go:348 component=tsdb msg="head GC completed" duration=227.952838ms
  level=info ts=2018-04-17T03:06:06.403176186Z caller=head.go:357 component=tsdb msg="WAL truncation completed" duration=2.74453888s
  level=info ts=2018-04-17T03:06:06.666573558Z caller=compact.go:394 component=tsdb msg="compact blocks" count=3 mint=1523901600000 maxt=1523923200000
  ```

- 查看 metrics 数据目录大小

    ```bash
    du -sh *

    89G prometheus2.0.0.data.metrics
    ```

- 查看 Prometheus 具体版本信息
  - 查看官方 changelog 信息，在 Prometheus 2.0.2 对启动速度有优化

  ```bash
  [tidb@Jeff deploy]$ ./prometheus --version
  prometheus, version 2.2.0 (branch: HEAD, revision: f63e7db4cbdb616337ca877b306b9b96f7f4e381)
    build user:       root@52af9f66ce71
    build date:       20180308-16:40:42
    go version:       go1.10
  ```

### 监控数据快速清理

- 节点下线后，metric 由残留数据
  - 第一种方案：
    - 停止 Prometheus 与 Push Gateway 服务，然后删除 Prometheus 的所有监控数据，然后启动两个组件。
  - 第二种方案：
    - 人工修改 metric ，按个过滤掉所有已经被下线的节点信息
    - histogram_quantile(0.99, sum(rate(tidb_server_handle_query_duration_seconds_bucket{instance!="6a5114505e5f_4000"}[1m])) by (le, instance))
  - 第三种方案：
    - 目前只能通过 http api 删除历史数据，不能删除 instance 信息
    - 且版本必须高于 2.1.0 以上，官方文档 `https://prometheus.io/docs/prometheus/latest/querying/api/#tsdb-admin-apis`
      - `curl -XPOST -g 'http://172.16.10.65:29090/api/v1/admin/tsdb/delete_series?match[]=tidb_server_handle_query_duration_seconds_bucket'`
      - `curl -XPOST -g 'http://172.16.10.65:29090/api/v1/admin/tsdb/clean_tombstones'`
