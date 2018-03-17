# 终端指引

### 安全设置

设备向服务器请求时，需要附加授权请求头，有两种方式：

GatewayToken方式      

* * "Authorization":"GatewayToken "+${gatewaytoken}

TargetToken方式

*  "Authorization":"TargetToken "+${targettoken}

注意GatewayToken和TargetToken后需空一格，且两个单词大小写敏感。

### 被动注册

除了使用系统管理后台主动注册设备外，设备可在配置GatewayToken的前提下被动注册：

![](/assets/Selection_015.png)

