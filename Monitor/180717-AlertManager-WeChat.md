# alertmanager 微信告警

## 调试 wechat

- https://work.weixin.qq.com/api/doc#10167/%E6%96%87%E6%9C%AC%E6%B6%88%E6%81%AF

获取以下信息

corp_id: wx9f383452d7edb40b
to_party: 1
agent_id: 1000030
api_secret: iEVrBXFkxdpJsFlDH6t9IhV6ApMsqyiKamypUh-2E9g


http://qydev.weixin.qq.com/wiki/index.php?title=主动调用

https://work.weixin.qq.com/api/doc#10649  全局错误码

gettoken


使用 token  sedmsg

wechat1='{   "touser" : "donghuanqing|shangliang",   "toparty" : "1",   "msgtype" : "text",   "agentid" : 1000030,   "text" : {       "content" : "this is msg"   },   "safe":0}'



curl -XPOST -d"$wechat1" https://qyapi.weixin.qq.com/cgi-bin/message/send?access_token=MjOltCSFvDqgPP2_9kAWIZk_SSuHEszhGF6VJTVwYl-TQt0aH4t6aNkasI-08hKa4M-U5gHfBWrVYvYUDiODm1zC4tVrIUJ6tMfJSuFiH0kDtbHOaiXVfvKrYPRtNdlXiXZdGzqu308x3G3UYyLTKf28oqiNO3ZiId7_8uyhG63QJouiz-2las7xbrVj5Bj3o3aDRRsPqgni70eCnW1flw

https://qyapi.weixin.qq.com/cgi-bin/gettoken?corpid=wx9f383452d7edb40b&corpsecret=iEVrBXFkxdpJsFlDH6t9IhV6ApMsqyiKamypUh-2E9g




{"errcode":0,"errmsg":"ok","access_token":"MjOltCSFvDqgPP2_9kAWIZk_SSuHEszhGF6VJTVwYl-TQt0aH4t6aNkasI-08hKa4M-U5gHfBWrVYvYUDiODm1zC4tVrIUJ6tMfJSuFiH0kDtbHOaiXVfvKrYPRtNdlXiXZdGzqu308x3G3UYyLTKf28oqiNO3ZiId7_8uyhG63QJouiz-2las7xbrVj5Bj3o3aDRRsPqgni70eCnW1flw","expires_in":7200}

## 构造 api 发送微信

curl -X POST -H 'Authorization: Bearer xoxb-1234-56789abcdefghijklmnop' \
-H 'Content-type: application/json' \
--data '{"channel":"looong","text":"I hope the tour went well, Mr. Wonka.","attachments": [{"text":"Who wins the lifetime supply of chocolate?","fallback":"You could be telling the computer exactly what it can do with a lifetime supply of chocolate.","color":"#3AA3E3","attachment_type":"default","callback_id":"select_simple_1234","actions":[{"name":"winners_list","text":"Who should win?","type":"select","data_source":"users"}]}]}' \
https://hooks.slack.com/services/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


## 模拟 告警

alerts1='[{  "labels": {       "alertname": "DiskRunningFull",       "dev": "sda1",       "instance": "example3",   "env": "tidb-sandbox"  ,   "severity": "warning"     }  }]'

curl -XPOST -d"$alerts1" http://10.110.24.2:9093/api/v1/alerts

[tidb@db-bj2-dbmgr-1 read-only]$ curl -XPOST -d"$alerts1" http://10.110.24.2:9093/api/v1/alerts && date
{"status":"success"}Tue Jul 17 14:25:14 CST 2018



## 调试 alertmanager to wechat

[wechat post](https://songjiayang.gitbooks.io/prometheus/content/alertmanager/wechat.html)

## 理解 

wechat config 代码块 https://github.com/prometheus/alertmanager/blob/2d3c4065e81ac6871cf01d72d92724dcd7474942/config/notifiers.go#L104

[wechat](https://github.com/prometheus/alertmanager/blob/a736a90dd031b4d2b2ee277346c2573961232e9f/notify/impl.go#L813 ) 获取 token 与 发送告警逻辑  813 to 975

09

### wechat 发送失败的告警

- 提示 corp_id 或者 api_secret 失败，根据代码逻辑判断 获取 token 失败

level=error ts=2018-07-17T07:14:15.322166012Z caller=dispatch.go:280 component=dispatcher msg="Notify for alerts failed" num_alerts=3 err="cancelling notify retry for \"wechat\" due to unrecoverable error: invalid APISecret for CorpID: wx9f383452d7edb40b"

### debug

```log
level=debug ts=2018-07-17T08:18:43.424128045Z caller=dispatch.go:445 component=dispatcher aggrGroup="{}/{env=~\"^(?:(tidb-sandbox|test-cluster))$\"}:{alertname=\"TiKV_memory_used_too_fast\", env=\"tidb-sandbox\", instance=\"10.30.1.9:9100\", job=\"overwritten-nodes\"}" msg=Flushing alerts=[TiKV_memory_used_too_fast[12b3991][active]]

level=debug ts=2018-07-17T08:18:43.424332247Z caller=impl.go:859 msg="Notifying Wechat" incident="{}/{env=~\"^(?:(tidb-sandbox|test-cluster))$\"}:{alertname=\"TiKV_memory_used_too_fast\", env=\"tidb-sandbox\", instance=\"10.30.1.9:9100\", job=\"overwritten-nodes\"}"

level=debug ts=2018-07-17T08:18:43.705580875Z caller=impl.go:952 msg="response: {\"errcode\":0,\"errmsg\":\"ok\",\"invaliduser\":\"\",\"invalidparty\":\"1\"}" incident="{}/{env=~\"^(?:(tidb-sandbox|test-cluster))$\"}:{alertname=\"TiKV_memory_used_too_fast\", env=\"tidb-sandbox\", instance=\"10.30.1.9:9100\", job=\"overwritten-nodes\"}"
```


```yml
global:
  slack_api_url: 'https://hooks.slack.com/services/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'

route:
  receiver: 'TiDB-Slack'
  group_by: ['env','instance','alertname','type','group','job']

  group_wait: 30s
  group_interval: 3m
  repeat_interval: 3m

  routes:

  - match_re:
      env: (tidb-sandbox|test-cluster)
    receiver: TiDB-Slack
    continue: true
  - match_re:
      env: (tidb-sandbox|test-cluster)
      level: emergency
    receiver: wechat

receivers:

- name: 'wechat'
  wechat_configs:
  - corp_id: 'wx9f383452d7edb40b'
    to_party: '1'
    agent_id: '1000030'
    api_secret: 'iEVrBXFkxdpJsFlDH6t9IhV6ApMsqyiKamypUh-2E9g'
    to_user: 'likai01|donghuanqing|shangliang|jiangxu'
    #to_user: 'donghuanqing|shangliang'
    #to_user: 'shangliang'

- name: 'TiDB-Slack'
  slack_configs:
  - channel: '#looong'
    username: '{{.CommonLabels.env}}'
    icon_emoji: ':{{.CommonLabels.env}}:'
    title:   '{{.CommonLabels.alertname}}'
    text:    'in {{.CommonLabels.env}}:   {{ .GroupLabels.alertname }}  {{ .CommonAnnotations.description }}'
```

## systemd

```bash
#!/bin/bash
set -e
ulimit -n 1000000

DEPLOY_DIR=/opt/tidb/alertmanager
cd "${DEPLOY_DIR}" || exit 1

exec >> (tee -i -a "/opt/tidb/alertmanager/log/alertmanager.log")
exec 2>&1

exec bin/alertmanager \
    --config.file="conf/mobike-alert.yml" \
    --storage.path="/opt/tidb/alertmanager/data" \
    --data.retention=120h \
    --alerts.gc-interval=30m \
    --log.level="info" \
    --web.listen-address=":9093"
```