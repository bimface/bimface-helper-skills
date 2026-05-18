# 文件夹重命名
```
PATCH https://api.bimface.com/bdfs/data/v1/projects/{projectId}/folders
```
### 说明
通过接口对文件夹的名称进行修改，可通过文件夹ID和文件夹路径两种参数指定需要修改的文件夹。

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
|name *|文件夹名称|string|
|path *|文件夹路径(folderId和path，必须二选一填入)|string|
|folderId *|文件夹ID(folderId和path，必须二选一填入)，ID的优先级大于path|string|

*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|RestResponse«FileItemDTO»|
|**204**|No Content|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|FileItemDTO|
|appKey|appKey|string|
|createTime|文件创建时间|string|
|current|是否为当前版本|boolean|
|fileId|文件ID|string|
|fileItemId|文件项ID|string|
|fileItemName|文件项名称|string|
|folder|是否为文件夹|boolean|
|id|文件夹ID|string|
|length|文件大小|int64|
|md5|md5|string|
|name|文件夹名称|string|
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
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/folders
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "folderId" : "1938888813662976",
  "name" : "BIM新文件夹"
}
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "data" : {
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2023-02-23 14:48:51",
    "id": "1938888813662976",
    "fileId": "1938888813662976",
    "fileItemId" : "1938888813662976",
    "name": "BIM新文件夹",
    "fileItemName": "BIM新文件夹",
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
    "updateTime" : "2023-02-24 14:01:39",
    "current": true,
    "originalCreateTime": "2023-02-23 14:48:51"
  },
  "message" : "success"
}
```


