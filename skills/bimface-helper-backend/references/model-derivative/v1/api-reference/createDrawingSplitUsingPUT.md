# 发起图纸拆分
```
PUT https://api.bimface.com/files/{fileId}/split
```


### 说明
通过文件ID创建图纸拆分


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
https://api.bimface.com/files/2078231483722272/split
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
  "message": null,
  "data" : {
    "createTime" : "2021-09-29 09:08:09",
    "databagVersion" : "4.2",
    "errorCode" : null,
    "length" : 0,
    "projectId" : "10000000006016",
    "reason" : null,
    "status" : "processing"
  }
}
```


