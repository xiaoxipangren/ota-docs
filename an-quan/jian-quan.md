# 鉴权

鉴权即身份识别，面对不同的功能模块，本系统提供了不同的鉴权机制。

## DDI API

考虑到终端设备的多样性、海量性和组网异构性，系统提供了以下四种终端鉴权方式。这四种鉴权方式可同时启用，只要终端满足其中任何一种便可视为鉴权通过。

#### 反向代理

允许终端通过由反向代理认证的证书进行认证。

#### 安全令牌

安全令牌是一个由系统为每个终端设备生成的32个字母数字的组合，终端可以通过Http认证头利用自定义的Scheme _TargetToken_来携带安全令牌达到身份是别的目的。

```js
GET /SPDEMO/controller/v1/0e945f95-9117-4500-9b0a-9c6d72fa6c07 HTTP/1.1
Host: your.ota.server
Authorization: TargetToken bH7XXAprK1ChnLfKSdtlsp7NOlPnZAYY
```

可通过系统管理界面中配置认证配置来启用安全令牌鉴权：

![](/assets/securitytoken.png)

#### 网关令牌

在很多场景下终端设备会通过一个网关间接地与OTA服务器进行交互，针对这种情况，系统提供了网关令牌的认证方式。网关令牌和安全令牌的形式一样，同为32个数字字母组合，不同的是，处于同一网关下的设备可以通过Http认证头携带相同的网关令牌，并将Scheme更改为_GatewayToken。_

```js
GET /SPDEMO/controller/v1/0e945f95-9117-4500-9b0a-9c6d72fa6c07 HTTP/1.1
Host: your.ota.server
Authorization: GatewayToken 3nkswAZhX81oDtktq0FF9Pn0Tc0UGXPW
```

可通过系统管理界面中配置认证配置来生成网关令牌并启用网关令牌鉴权：

![](/assets/gatewaytoken.png)

#### 匿名访问

为了测试需要，或者整个OTA系统处于受信任的网络环境中时，系统还提供了匿名鉴权的机制。

可通过系统管理界面中配置认证配置来启用匿名访问：

![](/assets/anonymous.png)

## Management API

用户通过Management API自行开发的管理程序通过Http Basic Auth即username/password进行鉴权

## Management UI

系统管理界面通过登录对话框提供username/password进行鉴权。

