# 更新集成信息
```
PATCH https://api.bimface.com/integrate
```


### 说明
模型集成发起后，可以通过该接口更新集成信息，如集成名称


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|integrateId *|集成ID|integer (int64)|
|name|新的集成名称|string|
*为必填项


### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«FileIntegrateBean»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|FileIntegrateBean|
|integrateId|集成ID|int64|
|sourceId|资源ID|string|
|databagId|数据包ID|string|
|reason|失败原因|string|
|thumbnail|缩略图|< object >array|
|errorCode|错误代码|string|
|priority|优先级|int32|
|type|文件类型|string|
|createTime|创建时间|string|
|name|集成文件名|string|
|appKey||string|
|projectId|项目ID|int64|
|status|任务状态|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/integrate?integrateId=1738888866720224&name="newName"
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

### HTTP响应示例
##### 响应 200
```json
{
  "code" : "success",
  "data" : {
    "appKey" : "appKey",
    "createTime" : "2017-12-25 17:25:25",
    "databagId" : "c444c81bc1bf4b048dfb759e0dc842a8",
    "errorCode" : "errorCode",
    "integrateId" : 1248789977538784,
    "name" : "newName",
    "priority" : 2,
    "projectId" : 0,
    "reason" : "reason",
    "sourceId" : "123156522123",
    "status" : "success",
    "thumbnail" : null,
    "type" : "type"
  },
  "message" : ""
}
```


