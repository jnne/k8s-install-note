**1.prometheus dingtalk 发送报警:**
-  promtheus 配置：

``` yaml
kind: ConfigMap
apiVersion: v1
metadata:
  name: alertmanager
  namespace: kube-system
data:
  config.yml: |-
    global:
      resolve_timeout: 5m
    route:
      receiver: webhook
      group_wait: 30s
      group_interval: 1m
      repeat_interval: 1m
      routes:
      - receiver: webhook
        group_wait: 10s
    receivers:
    - name: webhook
      webhook_configs:
      - url: 'http://10.255.72.206:80/' #这里地址是dingtalk的部署地址,后期可以通过k8s docker化成服务然后配置动态发现。
        send_resolved: true
```

- dingtalk_proxy.py
	> 这里是python2 python3的后期再补充。

``` python
#!/usr/bin/env python
#coding=utf-8
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
from flask import Flask
from flask import request
import json

app = Flask(__name__)

@app.route('/',methods=['POST'])

def send():
    if request.method == 'POST':
        post_data = request.get_data()
        post_data = json.loads(post_data)['alerts']
        for i in post_data:
                messa = ''' **%s** \n > %s \n > %s \n > %s ''' %(i['labels']['alertname'],i['startsAt'],i['annotations']['summary'],i['annotations']['description'])
                status = alert_data(messa,i['annotations']['summary'])
                print(messa,status)
    return status

def alert_data(data,title):
    from urllib2 import Request,urlopen
    url = 'https://oapi.dingtalk.com/robot/send?access_token=xxxxxxxxxxxxxxxx'
    send_data = '{"msgtype": "markdown","markdown": {"title": \"%s\" ,"text": \"%s\" }}' %(title,data)  # type: str
    send_data = send_data.encode('utf-8')
    request = Request(url, send_data)
    request.add_header('Content-Type','application/json')
    return urlopen(request).read()

if __name__ == '__main__':
    app.run(host='0.0.0.0',port="80")
```
