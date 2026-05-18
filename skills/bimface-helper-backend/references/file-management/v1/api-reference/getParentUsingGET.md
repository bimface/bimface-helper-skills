# 获取父文件夹
```
GET https://api.bimface.com/bdfs/data/v1/projects/{projectId}/folders/{folderId}/parent
```

### 说明
根据文件夹ID获取其上一层文件夹的信息，包含名称、创建时间等。


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|folderId *|文件夹ID|string|
|projectId *|项目ID|string|

*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|RestResponse«FileItemDTO»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|FileItemDTO|
|appKey|appKey|string|
|createTime|文件夹创建时间|string|
|current|是否为当前版本|boolean|
|fileId|文件ID|string|
|fileItemId|文件项ID|string|
|fileItemName|文件项名称|string|
|folder|是否为文件夹|boolean|
|id|文件夹ID|string|
|length|文件夹大小|int64|
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
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/folders/10000022260588/parent
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
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2023-02-23 14:48:51",
    "id": "10000022200497",
    "fileId": "10000022200497",
    "fileItemId" : "10000022200497",
    "name": "BIMFACE文件夹",
    "fileItemName": "BIMFACE文件夹",
    "suffix": null,
    "length": 0,
    "projectId" : "10000000006016",
    "parentId" : "10000000006016",    
    "folder": true,
    "storeId": null,
    "version": 1,
    "status": "SUCCESS",
    "uploadMode": null,
    "md5": null,
    "physicalIndex": null,           
    "updateTime" : "2023-02-23 14:48:51",
    "current": true,
    "originalCreateTime": "2023-02-23 14:48:51"
  },
  "message" : "success"
}
```


