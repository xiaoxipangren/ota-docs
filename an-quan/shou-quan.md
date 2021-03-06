# 授权

授权即判断一个对象是否拥有对资源的访问能力，如READ、WRITE等。

## DDI API

终端设备经鉴权后默认具有以下权限：

* 向服务器查询可执行的指令
* 向服务器提交更新反馈
* 下载其被赋予的更新内容

需要指出的是，如果系统启用了DDI API匿名认证，则未鉴权的设备也具有上述权限。

## Management API和UI

系统管理用户，无论是通过UI登录还是Basic Auth鉴权都具有相同的权限：

* CRUD 更新/模块/工件/标签
* 读取终端安全令牌
* 下载工件
* 更改系统配置，如鉴权机制、终端轮询间隔
* 监控终端设备状态



