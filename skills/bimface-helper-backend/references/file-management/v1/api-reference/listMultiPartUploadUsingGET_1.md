# 获取版本文件分片上传列表
```
GET https://api.bimface.com/bdfs/v1/data/projects/{project-id}/files/multi-part-files/upload-parts
```

### 说明
该接口可获取已经上传成功的版本文件的所有分片信息。

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
|id *|初始化分片任务时返回的ID|string|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«MultipartUploadSuccessPartListDTO»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|MultipartUploadSuccessPartListDTO|
|id|初始化分片上传时返回的id|string|
|successUploadParts|上传成功的分片信息|< PartETagInfo >array|
|partSize|分片文件大小|int64|
|etag|文件etag|string|
|partNumber|上传成功的分片序号|int32|
|message|提示信息|string|

### 消耗

* `application/json`


### 生成

* `application/json`
* `*/*`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/bdfs/v1/data/projects/string/files/multi-part-files/upload-parts?id=1938888813662976
```

##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

### HTTP响应示例

##### 响应 200
```json
{
  "code" : "string",
  "data" : {
    "id" : "1938888813662976",
    "successUploadParts" : [ {
      "etag" : "19349858cjs98ericu989",
      "partNumber" : 1,
      "partSize" : 100000
    } ]
  },
  "message" : null
}
```
