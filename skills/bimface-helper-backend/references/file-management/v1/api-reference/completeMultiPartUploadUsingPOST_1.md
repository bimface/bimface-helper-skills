
# 合并分片生成版本文件
```
POST https://api.bimface.com/bdfs/v1/data/projects/{project-id}/files/multi-part-files/merge
```

### 说明
在将所有数据Part都上传完成后，调用本接口来合并所有碎片，完成整个版本文件的分片上传。

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

##### Body
|名称|说明|类型|
|---|---|---|
|id *|初始化分片上传任务时返回的ID|string|

*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«FileItemDTOV1»|
|**201**|Created|-|
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
|name|版本文件名称|string|
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
https://api.bimface.com/bdfs/v1/data/projects/10000000006016/files/multi-part-files/merge
```

##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

##### 请求 body
```json
{
  "id" : "10000042160021"
}
```

### HTTP响应示例

##### 响应 200
```json
{
  "code" : "bimfaceservice-0000",
  "data" : {
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2023-09-27T07:53:28.408Z",
    "current" : true,
    "fileId" : "1938888813662004",
    "fileItemId" : "1938888813662976",
    "fileItemName" : "BIMFACE示例文件.rvt",
    "folder" : false,
    "id" : "1938888813662976",
    "length" : 100000,
    "md5" : "sdfhskbvnksdiuewriusbndskudf",
    "name" : "sample.rvt",
    "originalCreateTime" : "2022-02-02 02:02:02",
    "originalCreatorId" : "234234233234",
    "originalCreatorName" : "bimface",
    "parentId" : "10000000006016",
    "physicalIndex" : "sdfsdf/sdfjsdf/sdjjjj",
    "projectId" : "10000000006016",
    "status" : "success",
    "storeId" : "234234233234",
    "suffix" : "rvt",
    "updateTime" : "2022-02-02 02:02:02",
    "uploadMode" : "DIRECT",
    "version" : 2
  },
  "message" : null
}
```
