# 知之谓之 -- TiDB OPS

> That too late when it is the first time [.](https://www.google.com/ "Google Air Plan")

- bandwagonhost [affiliates](https://bandwagonhost.com/aff.php?aff=1572)

## TiDB Technology

- [TiDB Weekly](https://pingcap.com/weekly "Weekly update in TiDB")
- [TiDB Blog-cn List](TiDB-Blog-List.md) / TiDB 官方博文收集整理
- [The Third World](The-Third-World.md) / TiDB 周边技术文档收集

### Monitor

> 以下文档以 TiDB 监控架构为中心介绍两款开源组件，Prometheus 与 Grafana ，Prometheus 是一块时序数据库，Grafna 是一款数据展示工具。

- TiDB 监控架构
  - [Prometheus + Grafana = TiDB 业务监控架构](Monitor/170601-Prometheus-Grafana.md)
    - [Prometheus FAQ](Monitor/170606-Prometheus-FAQ.md)
    - [PushGateway FAQ](Monitor/18)
- 监控数据收集组件
  - [Node_exporter 主机性能状态收集](Monitor/170602-Node_exporter.md)
  - [Blackbox_exporter 主动监测主机与服务状态](Monitor/170603-Blackbox_exporter.md)
- 告警组件
  - [AlertManager 告警管理组件实际应用](Monitor/170607-AlertManager.md)
    - [AlertManager & Wechat 告警通知](Monitor/180717-AlertManager-WeChat.md)
- 告警与运维
  - [一条告警规则的定制 & 运维监控处理流程](Monitor/170605-Alert-Rules-Case.md)

### Docs

- [Syncer 选用守护进程工具](Docs/180323-Systemd-Syncer.md)
- [TiDB 有那些环境变量](Docs/180411-TiDB-vars.md)
- [TiSpark 服务安装 部署 测试](Docs/180416-TiSpark-deploy.md)
- [TiDB Cluster 修改组件 IP 地址](Docs/180327-TiDB-IP.md)

### Test

- [TiDB AUTO_INCREMENT 功能测试](Test/180327-AutoIncrementTest.md)
- [TiDB Cluster 使用域名部署集群](Test/180406-TiDB-Domain.md)
- [TiDB Online DDL 实操测试](Test/171010-TiDB-Online-DDL.md)

### Case-by-case

- [TiKV 磁盘空间回收步骤记录](Case-by-case/180503-Disk-Space-recovery.md)

### Troubleshooting

- [TiDB Binlog 部署以及问题处理](Troubleshooting/TiDB-Binlog.md)
- [TiDB-ansible 问题收集处理](Troubleshooting/tidb-ansible-FAQ.md)
- [Syncer 问题收集处理](Troubleshooting/Syncer-FAQ.md) -- ★

### Software

- [Tmux 快捷键 & 速查表](SoftWare/tmux.md)
- [Screen 速查表](SoftWare/screen.md)
- [NextCloud -- 下一代网络云盘](SoftWare/nextcloud.md)
- [RAID 与 Megacli](SoftWare/Megacli.md)
- [HAproxy -- Load Balance Service](SoftWare/HAproxy.md)
- [Transfer.sh -- 文件共享服务](SoftWare/Transfer.sh.md)
- [tcpkill 工具介绍](SoftWare/tcpkill.md)

### API

- TiDB [http_status.go](https://github.com/pingcap/tidb/blob/master/server/http_status.go) API 代码块
  - TiDB [API](https://github.com/pingcap/tidb/blob/master/docs/tidb_http_api.md) 使用文档
- PD [router](https://github.com/pingcap/pd/blob/master/server/api/router.go)
  - [PD Control](https://github.com/pingcap/docs-cn/blob/master/tools/pd-control.md) 使用说明
  - 使用 [PD Control](https://github.com/pingcap/docs-cn/blob/master/op-guide/horizontal-scale.md) 对 TIDB 集群扩容缩容案例

## About

> These violent delights have violent ends [.](https://github.com/BigerCAP/tidb-ops "Westworld")

![TiDB SQL at SCALE](/Media/about-logo.png "A Distributed HTAP database compatible with the MySQL protocol")
