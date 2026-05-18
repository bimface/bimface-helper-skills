# 获取指定版本文件信息
```
GET https://api.bimface.com/bdfs/data/v1/projects/{projectId}/files/{fileId}/meta
```

### 说明

根据文件ID获取指定版本的文件信息。

### 参数

##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|fileId *|文件ID|string|
|projectId *|项目ID|string|

*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|withFileSource|是否携带itemSource (默认值:"false")|boolean|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«FileItemDTOV1»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|FileItemDTOV1|
|appKey|appKey|string|
|createTime|文件创建时间|string|
|current|是否为当前版本|boolean|
|fileId|文件ID|string|
|fileItemId|文件项ID|string|
|fileItemName|文件项名称|string|
|folder|是否为文件夹|boolean|
|id|文件ID|string|
|length|文件大小|int64|
|md5|md5|string|
|name|文件名称|string|
|originalCreateTime|文件项创建时间|string|
|parentId|父文件夹ID|string|
|physicalIndex|对象存储索引|string|
|projectId|项目ID|string|
|status|文件状态|string|
|storeId|内部存储唯一标识|string|
|suffix|文件后缀|string|
|updateTime|文件更新时间|string|
|uploadMode|上传模式|string|
|version|文件版本号|int32|
|message|提示信息|string|

### 消耗

* `application/json`


### 生成

* `application/json`
* `*/*`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/bdfs/data/v1/projects/10000022120160/files/10000022340099/meta
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
  "data" : {
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime": "2023-03-15 13:18:47",
    "id": "10000022340099",
    "fileId": "10000022340099",
    "fileItemId": "10000022260842",
    "name": "示例文件.rvt",
    "fileItemName": "BIMFACE示例文件.rvt",
    "suffix": "rvt",
    "length": 6459392,
    "projectId": "10000022120160",
    "parentId": "10000022200497",
    "folder": false,
    "storeId": "10000022340097",
    "version": 5,
    "status": "SUCCESS",
    "uploadMode": "GENERAL",
    "md5": "af02c0906f369ae2029ef04afdb1913f",
    "physicalIndex": "d03821c0d4eb4416ae01e2a03723a32b",    
    "updateTime": "2023-03-15 13:18:47",
    "current": true,
    "originalCreateTime": "2023-03-08 11:08:50"
  }
}
```


