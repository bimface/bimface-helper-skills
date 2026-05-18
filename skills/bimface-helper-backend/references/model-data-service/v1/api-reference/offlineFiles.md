# 离线数据包
## 创建单文件的离线数据包
```
PUT https://api.bimface.com/files/{fileId}/offlineDatabag
```

### 说明
通过文件ID创建离线数据包

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|fileId *|文件ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|callback|回调url|string|


### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«DatabagDerivativeBean»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|返回数据|DatabagDerivativeBean|
|reason|失败原因|string|
|createTime|创建时间|string|
|length|数据包大小|int64|
|errorCode|错误码|string|
|databagVersion|数据包版本|string|
|projectId|项目ID|int64|
|status|任务状态|string|
|message|提示消息|string|

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/files/1938263713662976/offlineDatabag
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


### HTTP响应示例

##### 响应 200
```json
{
  "code": "success",
  "message": null,
  "data": {
    "createTime": "2021-10-17 22:35:45",
    "databagVersion": "3.7",
    "errorCode": null,
    "fileId": 1938888813662976,
    "length": 0,
    "projectId": "10000000006016",
    "reason": null,
    "status": "processing"
  }
}
```


## 查询单文件的离线数据包
```
GET https://api.bimface.com/files/{fileId}/offlineDatabag
```


### 说明
通过文件ID查询离线数据包


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|fileId *|文件ID|integer (int64)|
*为必填项


### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«DatabagDerivativeBean»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|返回数据|< DatabagDerivativeBean >array|
|reason|失败原因|string|
|createTime|创建时间|string|
|length|数据包大小|int64|
|errorCode|错误码|string|
|databagVersion|数据包版本|string|
|projectId|项目ID|int64|
|status|任务状态|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/files/1938263713662976/offlineDatabag
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


### HTTP响应示例

##### 响应 200
```json
{
  "code": "success",
  "message": null,
  "data": {
    "createTime": "2021-10-17 22:35:45",
    "databagVersion": "3.7",
    "errorCode": null,
    "fileId": 1938888813662976,
    "length": 7264664,
    "projectId": "10000000006016",
    "reason": null,
    "status": "success"
  }
}
```


## 创建集成模型的离线数据包
```
PUT https://api.bimface.com/integrations/{integrateId}/offlineDatabag
```


### 说明
通过集成ID创建离线数据包


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|integrateId *|集成ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|callback|回调url|string|


### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«DatabagDerivativeBean»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|返回数据|DatabagDerivativeBean|
|reason|失败原因|string|
|createTime|创建时间|string|
|length|数据包大小|int64|
|errorCode|错误码|string|
|databagVersion|数据包版本|string|
|projectId|项目ID|int64|
|status|任务状态|string|
|message|提示消息|string|

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/integrations/1738888866720224/offlineDatabag
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


### HTTP响应示例

##### 响应 200
```json
{
  "code": "success",
  "message": null,
  "data": {
    "createTime": "2021-10-17 22:53:30",
    "databagVersion": "3.5",
    "errorCode": null,
    "integrateId": 1738888866720224,
    "length": 0,
    "projectId": "10000000006016",
    "reason": null,
    "status": "processing"
  }
}
```


## 查询集成模型的离线数据包
```
GET https://api.bimface.com/integrations/{integrateId}/offlineDatabag
```


### 说明
通过集成ID查询离线数据包


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|integrateId *|集成ID|integer (int64)|
*为必填项


### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«DatabagDerivativeBean»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|返回数据|< DatabagDerivativeBean >array|
|reason|失败原因|string|
|createTime|创建时间|string|
|length|数据包大小|int64|
|errorCode|错误码|string|
|databagVersion|数据包版本|string|
|projectId|项目ID|int64|
|status|任务状态|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/integrations/1738888866720224/offlineDatabag
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


### HTTP响应示例

##### 响应 200
```json
{
  "code": "success",
  "message": null,
  "data": [
    {
      "createTime": "2021-10-17 22:53:31",
      "databagVersion": "3.5",
      "errorCode": null,
      "integrateId": 1738888866720224,
      "length": 7013573,
      "projectId": "10000000006016",
      "reason": null,
      "status": "success"
    }
  ]
}
```


## 创建对比文件的离线数据包
```
PUT https://api.bimface.com/comparisions/{compareId}/offlineDatabag
```


### 说明
通过对比ID创建离线数据包


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|compareId *|对比ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|callback|回调url|string|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«DatabagDerivativeBean»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|返回数据|DatabagDerivativeBean|
|reason|失败原因|string|
|createTime|创建时间|string|
|length|数据包大小|int64|
|errorCode|错误码|string|
|databagVersion|数据包版本|string|
|projectId|项目ID|int64|
|status|任务状态|string|
|message|提示消息|string|

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/comparisions/2077707858585728/offlineDatabag
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

### HTTP响应示例

##### 响应 200
```json
{
  "code": "success",
  "message": null,
  "data": {
    "compareId": 2077707858585728,
    "createTime": "2021-10-17 23:05:18",
    "databagVersion": "3.1",
    "errorCode": null,
    "length": 0,
    "projectId": "10000000006016",
    "reason": null,
    "status": "processing"
  }
}
```


## 查询对比文件的离线数据包
```
GET https://api.bimface.com/comparisions/{compareId}/offlineDatabag
```


### 说明
通过对比ID查询离线数据包


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|compareId *|对比ID|integer (int64)|
*为必填项


### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«DatabagDerivativeBean»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|返回数据|< DatabagDerivativeBean >array|
|reason|失败原因|string|
|createTime|创建时间|string|
|length|数据包大小|int64|
|errorCode|错误码|string|
|databagVersion|数据包版本|string|
|projectId|项目ID|int64|
|status|任务状态|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/comparisions/2077707858585728/offlineDatabag
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


### HTTP响应示例

##### 响应 200
```json
{
  "code": "success",
  "message": null,
  "data": [
    {
      "compareId": 2077707858585728,
      "createTime": "2021-10-17 23:05:18",
      "databagVersion": "3.1",
      "errorCode": null,
      "length": 1612046,
      "projectId": "10000000006016",
      "reason": null,
      "status": "success"
    }
  ]
}
```


## 获取数据包下载地址
```
GET https://api.bimface.com/data/databag/downloadUrl
```


### 说明
支持获取单模型，集成模型，模型对比的离线数据包，请求参数必须根据模型类型指定文件ID，集成ID或对比ID。如果都为空，则报错。


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|compareId|模型对比ID|integer (int64)|
|databagVersion|数据包版本；对于offline、vr数据包，如果只有一个，则下载唯一的数据包，如果多个，则必须指定数据包版本|string|
|fileId|代表单模型的文件ID|integer (int64)|
|integrateId|集成模型ID|integer (int64)|
|projectId|模型文件所属的project|integer (int64)|
|type|数据包类型，如offline、vr、igms|string|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GetUrlSwaggerDisplay|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|样例: "http://m.bimface.com/xxx.zip"|string|
|message||string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/databag/downloadUrl
```


##### 请求 header
```json
"Authorization: Bearer dc671840-bacc-4dc5-a134-97c1918d664b"
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "message" : "string",
  "data" : "http://m.bimface.com/xxx.zip"
}
```


