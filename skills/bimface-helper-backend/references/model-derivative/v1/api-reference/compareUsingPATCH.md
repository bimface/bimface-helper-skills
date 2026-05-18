# 更新对比信息
```
PATCH https://api.bimface.com/v2/compare
```


### 说明
文件对比发起后，可以通过该接口更新对比信息，如对比名称


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|compareId *|对比ID|integer (int64)|
|name|新的对比名称|string|
*为必填项


### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«ModelCompareBean»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|返回数据|ModelCompareBean|
|sourceId|调用方的文件源ID|string|
|workerType|worker类型|string|
|reason|失败原因|string|
|thumbnail|缩略图|< object >array|
|cost|耗时|int32|
|errorCode|错误码|string|
|compareId|对比ID|int64|
|priority|优先级|int32|
|type|文件类型|string|
|offlineDatabagStatus|离线数据包状态|string|
|createTime|创建时间|string|
|name|对比名称|string|
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
https://api.bimface.com/v2/compare?compareId=2077707858585728&name="newName"
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
    "compareId" : 2077707858585728,
    "cost" : null,
    "createTime" : "2021-12-25 16:17:27",
    "errorCode" : null,
    "name" : "newName",
    "offlineDatabagStatus" : "prepare",
    "priority" : 2,
    "projectId" : 10000000006016,
    "reason" : null,
    "sourceId" : "d4649ee227e345c8b7f0022342247dec",
    "status" : "success",
    "thumbnail" : null,
    "type" : "rvt",
    "workerType" : "null"
  },
  "message" : ""
}
```