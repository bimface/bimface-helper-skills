# 发起文件对比
```
POST https://api.bimface.com/v2/compare
```

### 说明
不同版本的模型或图纸在转换成功后，即可发起文件对比。由于对比需要一定时间，可以通过[Callback机制](https://bimface.com/docs/model-derivative/v1/developers-guide/callback.html)或查询接口获取对比状态，更多文件对比的说明可参考[开发指南](https://bimface.com/docs/model-derivative/v1/developers-guide/file-compare.html)。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|sourceId|调用方的文件源ID|string|
|previousId *|修改前的文件ID|int64|
|followingId *|修改后的文件ID|int64|
|name|对比名称|string|
|callback|回调地址|string|
|comparedEntityType *|对比类型，"file"为单文件对比，"integration"为集成模型对比|string|
|priority|优先级|int32|
|config|参数|object|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«ModelCompareBean»|
|**201**|Created|-|
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
https://api.bimface.com/v2/compare
```

##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

##### 请求 body
```json
{
  "callback" : "https://api.glodon.com/viewing/callback?authCode=BJ90Jk0affae&signature=2ef131395fb6442eb99abd83d45c2412",
  "comparedEntityType" : "file",
  "followingId" : 1938263713662977,
  "name" : "compare0001",
  "previousId" : 1938263713662976,
  "priority" : 2,
  "sourceId" : "d4649ee227e345c8b7f0022342247dec"
}
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
    "name" : "compare0001",
    "offlineDatabagStatus" : "prepare",
    "priority" : 2,
    "projectId" : 10000000006016,
    "reason" : null,
    "sourceId" : "d4649ee227e345c8b7f0022342247dec",
    "status" : "processing",
    "thumbnail" : null,
    "type" : "rvt",
    "workerType" : "null"
  },
  "message" : ""
}
```
