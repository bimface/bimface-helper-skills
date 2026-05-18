# 文件重命名
```
PATCH https://api.bimface.com/bdfs/data/v1/projects/{projectId}/fileItems
```

### 说明
通过接口对文件项的名称进行修改，可通过文件项ID和文件项所在路径两种参数指定需要修改的文件项。

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
|path *|文件项所在路径（fileItemId和path，必须二选一填入）|string|
|fileItemId *|文件项ID（fileItemId和path，必须二选一填入）|string|
|fileItemName *|新的文件项名称|string|
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
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/fileItems
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "fileItemId" : "1938888813662955",
  "fileItemName" : "sample2.rvt"
}
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "data" : {
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2023-02-27 12:25:49",
    "id": "10000022264947",
    "fileId": "10000022264947",
    "fileItemId" : "1938888813662955",
    "name": "BIMFACE示例文件.rvt",
    "fileItemName": "sample2.rvt",
    "suffix" : "rvt",
    "length" : 345345345,
    "projectId" : "10000000006016",
    "parentId" : "10000022200497",
    "folder": false,
    "storeId": "10000022264945",
    "version": 3,
    "status": "SUCCESS",
    "uploadMode": "DIRECT",
    "md5": "6DFB020D5761B66DA004DCF8CC09F193",
    "physicalIndex": "8f2379bbf6c8460aa5929a86642720e9",  
    "updateTime" : "2023-02-28 09:32:54",
    "current": true,
    "originalCreateTime": "2023-02-24 14:55:38"
  },
  "message" : "success"
}
```


