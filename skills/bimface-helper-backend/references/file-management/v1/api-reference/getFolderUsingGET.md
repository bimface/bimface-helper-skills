# 获取文件夹信息
```
GET https://api.bimface.com/bdfs/data/v1/projects/{projectId}/folders
```

### 说明
根据文件夹ID或文件夹路径，可获取文件夹信息，包含文件夹名称、创建时间等。

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
|folderId *|文件夹ID（folderId和path，必须二选一填入）|string|
|path *|文件夹所在路径，使用URL编码UTF-8，最多256个字符（folderId和path，必须二选一填入）|string|
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
|createTime|文件创建时间|string|
|current|是否为当前版本|boolean|
|fileId|文件ID|string|
|id|文件夹ID|string|
|fileItemId|文件项ID|string|
|fileItemName|文件项名称|string|
|folder|是否为文件夹|boolean|
|length|文件夹大小|int64|
|md5|md5|string|
|name|文件夹名称|string|
|originalCreateTime|文件夹创建时间|string|
|parentId|父文件夹ID|string|
|physicalIndex|对象存储索引|string|
|projectId|项目ID|string|
|status|文件状态|string|
|storeId|内部存储唯一标识|string|
|suffix|文件后缀|string|
|updateTime|文件夹更新时间|string|
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
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/folders?folderId=1938888813662976
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
    "createTime" : "2022-02-02 02:02:02",
    "id": "1938888813662976",
    "fileId": "1938888813662976",
    "fileItemId" : "1938888813662976",
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
    "updateTime" : "2022-02-02 02:02:02",
    "current": true,
    "originalCreateTime": "2022-02-02 02:02:02"
  },
  "message" : "success"
}
```
