# 微信Api/WeChatApi
基于windows平台开发的微信Api,仅支持2.9.0.122版本。
Api程序入群自取，现免费。
## [文档](https://frz2one.github.io/Api.html)
## Demo

### Python

> 自动同意好友并且回复内容，关键词拉人进群demo

```python
import requests
import  json
#import qrcode
import random
import threading
import time
import websocket
# -*- coding: utf-8 -*-

x="""回复12300邀请进狗子微信Api交流群"""#同意好友回复内容
dict0={"12300":"12312312388@chatroom"}#关键词邀请字典
#初始化
url = "http://127.0.0.1:2222/"
header = {"Content-Type": "application/json"}
body_1 = {"funid": 1, "key": "填写的你key"}
body_1 = str(body_1)
body_1 = bytes(body_1, encoding="utf-8")
response_1 = requests.post(url, data=body_1, headers=header)
print(response_1.text)
#接口
def WeChatPost(dict):
    url = "http://127.0.0.1:2222/"
    header = {"Content-Type": "application/json"}
    body_1 =dict
    body_1 = str(body_1)
    body_1 = bytes(body_1, encoding="utf-8")
    response_1 = requests.post(url, data=body_1, headers=header)
    return response_1.content.decode('utf-8')
    pass
def Agreed(message):
    message0=message["content"]
    print(message0)
    WeChatPost({"funid": 51, "WeChatID": message["WeChatID"], "v1": message0[message0.find("encryptusername=") + 17:message0.find("fromnickname") - 2],"v4": message0[message0.find("ticket=") + 8:message0.find("opcode=") - 2]})
    time.sleep(2)
    WeChatPost({"funid":20,"WeChatID":message["WeChatID"],"wxid":message0[message0.find("fromusername") + 14:message0.find("fromusername") + 33],"content":x}) #发送同意好友回复
    pass
def msg(message):
    if message["wxid"].find("wxid") != -1 :
        for (key, value) in dict0.items():
            if key==message["content"] :
                WeChatPost({"funid": 32, "WeChatID": message["WeChatID"], "wxid": value,"wxidlist": [message["wxid"]]})
                pass
            pass
        pass
    pass

#Websocket回调和连接
def on_message(ws, message): #处理逻辑
    message_0 = json.loads(message)
    if message_0["type"]==1:
        threading.Thread(target=msg, args=(message_0,)).start()  # 线程执行消息处理函数

    if message_0["type"]==37: #好友申请
        print("好友申请")
        threading.Thread(target=Agreed, args=(message_0,)).start() #线程执行同意好友并回复消息函数
        pass

def on_error(ws, error):
    print(ws)
    print(error)
def on_close(ws):
    print(ws)
    print("### closed ###")
websocket.enableTrace(True)
ws = websocket.WebSocketApp("ws://127.0.0.1:2222/",on_message=on_message,on_error=on_error,on_close=on_close)
ws.run_forever()
```

## 更新日志

### 2020.6.21

1. 优化了消息处理逻辑（视频（`type:43`）和图片（`type:3`）返回文件的地址）
2. 简化json数据
3. 新增接收消息附带WeChatID标识符，多开更容易识别消息来源微信
4. 登录设置禁止更新，避免新版本自动更新

# 交流
添加微信进群：
<img src="https://cdn.jsdelivr.net/gh/frz2one/wechatapi/qrcode.jpg" alt="jpg.jpp" style="zoom:50%;" />
