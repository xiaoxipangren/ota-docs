# Management API

### 概述

Management API是一组Restful风格的满足HATEOAS规范的用于CRUD终端和软件资源的接口，可通过Http/Https使用JSON携带超媒体资源，并支持过滤、排序和分页功能。API使用前需要进行鉴权和授权操作。

### API版本

API通过版本控制支持向下的兼容性，目前的版本是v1。

### API资源

支持以下四种Http方法：

* GET
* POST
* PUT
* DELETE

详细的API文档如下：



### Http头

所有的Http请求应当包含用于Basic Auth的Authorization请求头：

* Username:username
* Password:password

对于POST和PUT请求应当设置Content-Type头，可选值有：

* application/json
* application/hal+json

### 响应体

除了相关的自身属性数据，每个实体资源还有包含有指向其相关的其他实体的链接，如一个更新实体会包含相关的工件、模块、模块类型、元数据的链接：

```js
"_links": {
    "artifacts": {
        "href": "http://localhost:8080/rest/v1/softwaremodules/83/artifacts"
    },
    "self": {
        "href": "http://localhost:8080/rest/v1/softwaremodules/83"
    },
    "type": {
        "href": "http://localhost:8080/rest/v1/softwaremoduletypes/43"
    },
    "metadata": {
        "href": "http://localhost:8080/rest/v1/softwaremodules/83/metadata?offset=0&limit=50"
    }
```



