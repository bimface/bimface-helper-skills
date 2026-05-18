# 获取文件状态
```
GET https://api.bimface.com/bdfs/data/v1/projects/{projectId}/fileItems/status
```

### 说明
根据文件项ID或文件项所在路径获取文件上传状态信息，此时获取的是该文件项的当前版本的信息。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|projectId *|项目ID|string|

*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|fileItemId *|文件项ID（fileItemId和path，必须二选一填入）|string|
|path *|文件项所在路径，使用URL编码（UTF-8），最多256个字符（fileItemId和path，必须二选一填入）|string|

*为必填项


### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|RestResponse«FileItemUploadStatusBean»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|FileItemUploadStatusBean|
|fileItemId|文件项ID|string|
|fileItemName|文件项名称|string|
|name|文件名称|string|
|failedReason|失败原因|string|
|status|文件状态|string|
|message|提示信息|string|

### 消耗

* `application/json`


### 生成

* `application/json`
* `*/*`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/fileItems/status?fileItemId=1938888813662976
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
    "fileItemId": "1938888813662976",
    "name": "sample.rvt",
    "fileItemName": "Revit示例模型.rvt",
    "status": "SUCCESS",
    "failedReason": null
  },
  "message" : "success"
}
```


