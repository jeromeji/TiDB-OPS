---
title: HAProxy
date: 2017-06-10 21:40:52
updated: 2017-06-10 21:40:52
categories:
  - software
tags:
  - TiDB
  - HAProxy
  - Air
---
# HAProxy

- [官网](http://www.haproxy.org)

## 功能

- 负载均衡：L4(TCP) 和 L7(HTTP/HTTPS) 两种模式，支持 RR(roundrobin) / 静态 RR / LC / IP Hash/ URI Hash/ URL_PARAM Hash/ HTTP_HEADER Hash 等丰富的负载均衡算法
- 健康检查：支持 TCP 和 HTTP 两种健康检查模式
- 会话保持：对于未实现会话共享的应用集群，可通过 Insert Cookie / Rewrite Cookie / Prefix Cookie，以及上述的多种 Hash 方式实现会话保持
- SSL：HAProxy 可以解析 HTTPS 协议，并能够将请求解密为 HTTP 后向后端传输
- HTTP：请求重写与重定向
- 监控与统计：HAProxy 提供了基于 Web 的统计信息页面，展现健康状态和流量数据。基于此功能，使用者可以开发监控程序来监控 HAProxy 的状态

## 配置文件

> TiDB or MySQL 需要使用 L4 (OSI) TCP 层模式

- 使用以下配置文件，可以通过 `http://hostIP:1111` 查看 HAProxy 运行状态

```configuration
#---------------------------------------------------------------------
# Example configuration for a possible web application.  See the
# full configuration options online.
#
#   http://haproxy.1wt.eu/download/1.4/doc/configuration.txt
#
#---------------------------------------------------------------------

#---------------------------------------------------------------------
# Global settings
#---------------------------------------------------------------------
global
    # to have these messages end up in /var/log/haproxy.log you will
    # need to:
    #
    # 1) configure syslog to accept network log events.  This is done
    #    by adding the '-r' option to the SYSLOGD_OPTIONS in
    #    /etc/sysconfig/syslog
    #
    # 2) configure local2 events to go to the /var/log/haproxy.log
    #   file. A line like the following can be added to
    #   /etc/sysconfig/syslog
    #
    #    local2.*                       /var/log/haproxy.log
    #
    log         127.0.0.1 local2

    chroot      /var/lib/haproxy
    pidfile     /var/run/haproxy.pid
    maxconn     4000
    user        haproxy
    group       haproxy
    daemon

    # turn on stats unix socket
    stats socket /var/lib/haproxy/stats

defaults
    log global
    retries 2
  timeout connect  2s   #连接超时
  timeout client 30000s    #客户端超时
  timeout server 30000s    #服务器超时


listen admin_stats
  bind 0.0.0.0:1111
  mode http
  option httplog                  #采用 http 日志格式
  maxconn 4000                    #默认的最大连接数
  stats refresh 30s               #统计页面自动刷新时间
  stats uri /haproxy              #统计页面 url
  stats realm Haproxy             #统计页面密码框上提示文本
  stats auth admin:pingcap123     #设置监控页面的用户和密码: admin, 可以设置多个用户名
  stats hide-version              #隐藏统计页面上 HAProxy 的版本信息
  stats  admin if TRUE            #设置手工启动 / 禁用，后端服务器 (haproxy-1.4.9 以后版本)


listen tidb-cluster
    bind 0.0.0.0:3306
    mode tcp
    balance roundrobin
    server tidb-1 172.16.18.11:4000 check inter 2000 rise 2 fall 3
    server tidb-2 172.16.18.12:4000 check inter 2000 rise 2 fall 3
    server tidb-3 172.16.18.13:4000 check inter 2000 rise 2 fall 3
```