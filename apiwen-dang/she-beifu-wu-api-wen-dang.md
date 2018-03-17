# DDI API

DDI API基于Http标准和轮询机制。

DDI API使用controllerId这一术语来进行终端身份识别。

## 状态机映射

由于历史原因，DDI API和终端拥有不同的状态机和状态信息。为了确保DDI和终端保持兼容定义此状态映射。

The\_DDI\_API allows the device to provide the following feedback messages:

| DDI 执行状态 | 说明 | 映射的Action类型 |
| :--- | :--- | :--- |
| CANCELED | 该状态是作为服务区升级取消请求的确认由终端发送给服务器 | CANCELED |
| REJECTED | 当升级由于某些原因无法执行时由终端向服务端发送 | WARNING |
| CLOSED | 当终端执行升级确定成功或者失败时向服务器发送 | ERROR \(DDI FAILURE\) or FINISHED \(DDI SUCCESS or NONE\) |
| PROCEEDING | 终端正处于升级中 | RUNNING |
| SCHEDULED | 终端已计划进行升级 | RUNNING |
| RESUMED | 终端使用此状态说明其可继续执行升级 | RUNNING |

## API 概述

接下来的章节针对最重要的API接口提供了基本额s示例，详细的接口说明在[这里](https://docs.bosch-iot-rollouts.com/documentation/rest-api/rootcontroller-api-guide.html)。

## 轮询接口 {#base-poll-resource}

```js
GET /{tenant}/controller/v1/{controllerId}
```

服务器在轮询接口的应答中会加入动作链接。一个可能的动作链接是一个部署命令：

_响应示例_

```
{
    "config": {
        "polling": {
            "sleep": "00:05:00"
        }
    },
    "_links": {
        "deploymentBase": {
            "href": "http://localhost:8080/default/controller/v1/example/deploymentBase/1?c=644088541"
        },
        "configData": {
            "href": "http://localhost:8080/default/controller/v1/example/configData"
        }
    }
}
```

## 升级接口 {#deployment-base-resource}

```
GET /{tenant}/controller/v1/{controllerId}/deploymentBase/{id}
```

_响应示例_

```
{
    "deployment": {
        "download": "forced",
        "update": "forced",
        "chunks": [
            {
                "part": "os",
                "version": "1.0.0",
                "name": "Linux",
                "artifacts": [
                    {
                        "filename": "linux.zip",
                        "hashes": {
                            "sha1": "46fc56de883ec027759d8513458fe1010aa7e793",
                            "md5": "5813e9655bd6871d0c25b8d510fd8605"
                        },
                        "size": 52167,
                        "_links": {
                            "download": {
                                "href": "http://localhost:8080/default/controller/v1/example/softwaremodules/1/artifacts/linux.zip"
                            },
                            "md5sum": {
                                "href": "http://localhost:8080/default/controller/v1/example/softwaremodules/1/artifacts/linux.zip.MD5SUM"
                            }
                        }
                    }
                ]
            }
        ]
    },
    "id": "1",
    "actionHistory": {
        "status": "RETRIEVED",
        "messages": [
            "Installing update",
            "Downloading artifacts"
            ]
    }
}
```

## 升级反馈接口 {#deployment-feedback-resource}

```
POST /{tenant}/controller/v1/{controllerId}/deploymentBase/{id}/feedback
Content-Type: application/json
```

升级成功反馈示例

```
{
    "id": 1,
    "time": "20140511T121314",
    "status": {
        "execution": "closed",
        "result": {
            "finished": "success",
            "progress": {}
        }
    }
}
```

正在升级反馈示例

```
{
    "id": "1",
    "time": "20140511T121314",
    "status": {
        "execution": "proceeding",
        "result": {
            "finished": "none",
            "progress": {
                "cnt": 2,
                "of": 5
            }
        },
        "details": [
            "checking hash sums"
        ]
    }
}
```

升级错误反馈示例

```
{
    "id": 1,
    "time": "20140511T121314",
    "status": {
        "execution": "rejected",
        "result": {
            "finished": "failure",
            "progress": {}
        },
        "details": [
            "something bad happend"
        ]
    }
}
```



