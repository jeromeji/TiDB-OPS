# 重新加载配置文件

kill -HUB PID

level=info ts=2018-07-17T03:25:06.928031371Z caller=main.go:320 msg="Loading configuration file" file=mobike-alert.yml
level=error ts=2018-07-17T03:25:06.928422864Z caller=main.go:323 msg="Loading configuration file failed" file=mobike-alert.yml err="yaml: unmarshal errors:\n  line 33: field username not found in type config.plain\n  line 34: field icon_emoji not found in type config.plain\n  line 35: field title not found in type config.plain\n  line 36: field text not found in type config.plain"

----

curl http://10.30.1.9:9090/-/reload/

不加载 alert rule配置文件信息

```Loading
level=info ts=2018-07-16T09:00:01.999595897Z caller=head.go:348 component=tsdb msg="head GC completed" duration=83.771047ms
level=info ts=2018-07-16T09:00:03.164901888Z caller=head.go:357 component=tsdb msg="WAL truncation completed" duration=1.165234407s
level=info ts=2018-07-16T09:00:03.255869312Z caller=compact.go:393 component=tsdb msg="compact blocks" count=3 mint=1531699200000 maxt=1531720800000
level=info ts=2018-07-16T11:00:00.050852667Z caller=compact.go:393 component=tsdb msg="compact blocks" count=1 mint=1531728000000 maxt=1531735200000
level=info ts=2018-07-16T11:00:01.929853119Z caller=head.go:348 component=tsdb msg="head GC completed" duration=83.775197ms
level=info ts=2018-07-16T11:00:03.255013809Z caller=head.go:357 component=tsdb msg="WAL truncation completed" duration=1.325081776s
level=warn ts=2018-07-16T11:01:29.08315446Z caller=main.go:374 msg="Received SIGTERM, exiting gracefully..."
level=info ts=2018-07-16T11:01:29.083248401Z caller=main.go:398 msg="Stopping scrape discovery manager..."
level=info ts=2018-07-16T11:01:29.083281796Z caller=main.go:411 msg="Stopping notify discovery manager..."
level=info ts=2018-07-16T11:01:29.083292274Z caller=main.go:432 msg="Stopping scrape manager..."
level=info ts=2018-07-16T11:01:29.08328341Z caller=main.go:394 msg="Scrape discovery manager stopped"
level=info ts=2018-07-16T11:01:29.083309674Z caller=main.go:407 msg="Notify discovery manager stopped"
level=info ts=2018-07-16T11:01:29.083801395Z caller=main.go:426 msg="Scrape manager stopped"
level=info ts=2018-07-16T11:01:29.094286439Z caller=manager.go:460 component="rule manager" msg="Stopping rule manager..."
level=info ts=2018-07-16T11:01:29.094348038Z caller=manager.go:466 component="rule manager" msg="Rule manager stopped"
level=info ts=2018-07-16T11:01:29.094362159Z caller=notifier.go:512 component=notifier msg="Stopping notification manager..."
level=info ts=2018-07-16T11:01:29.094375822Z caller=main.go:573 msg="Notifier manager stopped"
level=info ts=2018-07-16T11:01:29.094615302Z caller=main.go:584 msg="See you next time!"
level=info ts=2018-07-16T11:01:33.017188058Z caller=main.go:220 msg="Starting Prometheus" version="(version=2.2.1, branch=HEAD, revision=bc6058c81272a8d938c0
5e75607371284236aadc)"
level=info ts=2018-07-16T11:01:33.01724485Z caller=main.go:221 build_context="(go=go1.10, user=root@149e5b3f0829, date=20180314-14:15:45)"
level=info ts=2018-07-16T11:01:33.017263694Z caller=main.go:222 host_details="(Linux 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 10-30-1
-9 (none))"
level=info ts=2018-07-16T11:01:33.017278322Z caller=main.go:223 fd_limits="(soft=1000000, hard=1000000)"
level=info ts=2018-07-16T11:01:33.019449294Z caller=main.go:504 msg="Starting TSDB ..."
level=info ts=2018-07-16T11:01:33.019491071Z caller=web.go:382 component=web msg="Start listening for connections" address=:9090
level=info ts=2018-07-16T11:01:42.459431759Z caller=main.go:514 msg="TSDB started"
level=info ts=2018-07-16T11:01:42.459497294Z caller=main.go:588 msg="Loading configuration file" filename=/data1/deploy/conf/prometheus.yml
level=info ts=2018-07-16T11:01:42.469386474Z caller=main.go:491 msg="Server is ready to receive web requests."
level=info ts=2018-07-16T11:06:25.133852362Z caller=main.go:588 msg="Loading configuration file" filename=/data1/deploy/conf/prometheus.yml
level=warn ts=2018-07-16T11:10:07.132065358Z caller=main.go:374 msg="Received SIGTERM, exiting gracefully..."
level=info ts=2018-07-16T11:10:07.132148063Z caller=main.go:398 msg="Stopping scrape discovery manager..."
level=info ts=2018-07-16T11:10:07.132173902Z caller=main.go:411 msg="Stopping notify discovery manager..."
level=info ts=2018-07-16T11:10:07.132194129Z caller=main.go:432 msg="Stopping scrape manager..."
level=info ts=2018-07-16T11:10:07.132175445Z caller=main.go:394 msg="Scrape discovery manager stopped"
level=info ts=2018-07-16T11:10:07.132199605Z caller=main.go:407 msg="Notify discovery manager stopped"
level=info ts=2018-07-16T11:10:07.132906422Z caller=main.go:426 msg="Scrape manager stopped"
level=info ts=2018-07-16T11:10:07.135713409Z caller=manager.go:460 component="rule manager" msg="Stopping rule manager..."
level=info ts=2018-07-16T11:10:07.135762567Z caller=manager.go:466 component="rule manager" msg="Rule manager stopped"
level=info ts=2018-07-16T11:10:07.135776477Z caller=notifier.go:512 component=notifier msg="Stopping notification manager..."
level=info ts=2018-07-16T11:10:07.135799329Z caller=main.go:573 msg="Notifier manager stopped"
level=info ts=2018-07-16T11:10:07.136049686Z caller=main.go:584 msg="See you next time!"
level=info ts=2018-07-16T11:10:08.211730925Z caller=main.go:220 msg="Starting Prometheus" version="(version=2.2.1, branch=HEAD, revision=bc6058c81272a8d938c0
5e75607371284236aadc)"
level=info ts=2018-07-16T11:10:08.211791715Z caller=main.go:221 build_context="(go=go1.10, user=root@149e5b3f0829, date=20180314-14:15:45)"
level=info ts=2018-07-16T11:10:08.211810308Z caller=main.go:222 host_details="(Linux 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 10-30-1
-9 (none))"
level=info ts=2018-07-16T11:10:08.211825457Z caller=main.go:223 fd_limits="(soft=1000000, hard=1000000)"
level=info ts=2018-07-16T11:10:08.214414822Z caller=web.go:382 component=web msg="Start listening for connections" address=:9090
level=info ts=2018-07-16T11:10:08.21437633Z caller=main.go:504 msg="Starting TSDB ..."
level=warn ts=2018-07-16T11:10:18.732481499Z caller=head.go:320 component=tsdb msg="unknown series references in WAL samples" count=19754
level=info ts=2018-07-16T11:10:18.742567967Z caller=main.go:514 msg="TSDB started"
level=info ts=2018-07-16T11:10:18.742622753Z caller=main.go:588 msg="Loading configuration file" filename=/data1/deploy/conf/prometheus.yml
level=info ts=2018-07-16T11:10:18.752166388Z caller=main.go:491 msg="Server is ready to receive web requests."
level=info ts=2018-07-16T13:00:00.073875802Z caller=compact.go:393 component=tsdb msg="compact blocks" count=1 mint=1531735200000 maxt=1531742400000
level=info ts=2018-07-16T13:00:01.845980129Z caller=head.go:348 component=tsdb msg="head GC completed" duration=65.665159ms
level=info ts=2018-07-16T13:00:02.884631063Z caller=head.go:357 component=tsdb msg="WAL truncation completed" duration=1.038570175s
level=info ts=2018-07-16T15:00:00.075186945Z caller=compact.go:393 component=tsdb msg="compact blocks" count=1 mint=1531742400000 maxt=1531749600000
level=info ts=2018-07-16T15:00:01.999426227Z caller=head.go:348 component=tsdb msg="head GC completed" duration=83.582019ms
level=info ts=2018-07-16T15:00:03.181693366Z caller=head.go:357 component=tsdb msg="WAL truncation completed" duration=1.182188742s
level=info ts=2018-07-16T15:00:03.268541367Z caller=compact.go:393 component=tsdb msg="compact blocks" count=3 mint=1531720800000 maxt=1531742400000
level=info ts=2018-07-16T15:00:06.089412464Z caller=compact.go:393 component=tsdb msg="compact blocks" count=3 mint=1531677600000 maxt=1531742400000
level=info ts=2018-07-16T17:00:00.07474752Z caller=compact.go:393 component=tsdb msg="compact blocks" count=1 mint=1531749600000 maxt=1531756800000
```




```bash
[ops@10-30-1-9 conf]$ ps aux | grep prome
ops       47313  0.0  0.0 112644   952 pts/0    S+   11:41   0:00 grep --color=auto prome
ops      195028  0.0  0.0   1088     4 ?        S    Jul16   0:00 bin/supervise status/prometheus /data1/deploy/scripts/run_prometheus.sh
ops      195029 12.6  0.7 30260340 1042112 ?    Sl   Jul16 125:16 bin/prometheus --config.file=/data1/deploy/conf/prometheus.yml --web.listen-address=:9090 --web.external-url=http://10.30.1.9:9090/ --web.enable-admin-api --log.level=info --storage.tsdb.path=/data1/deploy/prometheus2.0.0.data.metrics --storage.tsdb.retention=15d
ops      195031  0.0  0.0 113116   632 ?        S    Jul16   0:00 /bin/bash /data1/deploy/scripts/run_prometheus.sh
ops      195032  0.0  0.0 107892   668 ?        S    Jul16   0:00 tee -i -a /data1/deploy/log/prometheus.log
[ops@10-30-1-9 conf]$ curl http://10.30.1.9:9090/-/reload/
<a href="/-/reload">Moved Permanently</a>.

[ops@10-30-1-9 conf]$ ps aux | grep prome
ops       47322  0.0  0.0 112644   948 pts/0    S+   11:41   0:00 grep --color=auto prome
ops      195028  0.0  0.0   1088     4 ?        S    Jul16   0:00 bin/supervise status/prometheus /data1/deploy/scripts/run_prometheus.sh
ops      195029 12.6  0.7 30260340 1042112 ?    Sl   Jul16 125:17 bin/prometheus --config.file=/data1/deploy/conf/prometheus.yml --web.listen-address=:9090 --web.external-url=http://10.30.1.9:9090/ --web.enable-admin-api --log.level=info --storage.tsdb.path=/data1/deploy/prometheus2.0.0.data.metrics --storage.tsdb.retention=15d
ops      195031  0.0  0.0 113116   632 ?        S    Jul16   0:00 /bin/bash /data1/deploy/scripts/run_prometheus.sh
ops      195032  0.0  0.0 107892   668 ?        S    Jul16   0:00 tee -i -a /data1/deploy/log/prometheus.log
[ops@10-30-1-9 conf]$ 
```

