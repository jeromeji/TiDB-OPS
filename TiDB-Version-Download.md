# TiDB Binary 下载地址

## TiDB

### 最新开发版

- TiDB-Master
  - http://download.pingcap.org/tidb-latest-linux-amd64.tar.gz

- Docker 版本
  - TiDB Docker Image: `uhub.service.ucloud.cn/pingcap/tidb:latest`
  - PD   Docker Image: `uhub.service.ucloud.cn/pingcap/pd:latest`
  - TiKV Docker Image: `uhub.service.ucloud.cn/pingcap/tikv:latest`

### Release

- TiDB Binary Release
  - http://download.pingcap.org/tidb-{version}-linux-amd64.tar.gz
    - {version} 可通过 TiDB repo tags 查看
  - http://download.pingcap.org/tidb-v2.1.2-linux-amd64.tar.gz
  - http://download.pingcap.org/tidb-v2.0.7-linux-amd64.tar.gz

- TiDB Docker Release
  - TiDB Docker Image: `pingcap/tidb:v2.0.5`
  - PD   Docker Image: `pingcap/pd:v2.0.5`
  - TiKV Docker Image: `pingcap/tikv:v2.0.5`

### 指定 Githash 下载指定组件

Githash 在 repo commit 历史记录可查到

- curl -SLO http://pingcap-dev.hk.ufileos.com/builds/pingcap/tidb/pr/${commit-hash}/centos7/tidb-server.tar.gz
- curl -SLO http://pingcap-dev.hk.ufileos.com/builds/pingcap/tikv/pr/${commit-hash}/unportable_centos7/tikv-server.tar.gz
- curl -SLO http://pingcap-dev.hk.ufileos.com/builds/pingcap/pd/pr/${commit-hash}/centos7/pd-server.tar.gz

### 安装部署工具

- tidb-ansible / 安装部署工具
  - https://github.com/pingcap/tidb-ansible
- DM-ansible / DM 同步工具
  - http://download.pingcap.org/dm-ansible-latest.tar.gz
- Kafka-ansible
  - https://github.com/pingcap/thirdparty-ops

## TiDB-enterprise-tools

- TiDB-bench
  - sysbench 压测工具
  - https://github.com/pingcap/tidb-bench

- TiDB-binlog
  - TiDB binlog / gRPC
  - http://download.pingcap.org/tidb-binlog-latest-linux-amd64.tar.gz

- syncer / loader / mydumper
  - 数据导入导出/同步工具
  - http://download.pingcap.org/tidb-enterprise-tools-latest-linux-amd64.tar.gz

- TiDB-tools
  - 周边工具
  - http://download.pingcap.org/tidb-tools-latest-linux-amd64.tar.gz

- reparo
  - PB 还原 SQL 工具
  - https://download.pingcap.org/reparo-latest-linux-amd64.tar.gz

- sync_check
  - 数据一致性检测工具
  - https://download.pingcap.org/sync-check-latest-linux-amd64.tar.gz

- lightning
  - https://download.pingcap.org/tidb-lightning-latest-linux-amd64.tar.gz
  - https://download.pingcap.org/tidb-lightning-release-2.0-linux-amd64.tar.gz

## 运维排查工具

- 运维
  - https://github.com/pingcap/tidb-inspect-tools
- 火焰图
  - http://139.219.11.38:3000/UmFjU/FlameGraph-master.zip
  - https://github.com/brendangregg/FlameGraph/archive/master.zip