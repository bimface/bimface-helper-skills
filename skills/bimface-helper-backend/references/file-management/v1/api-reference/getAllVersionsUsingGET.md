# 获取所有版本文件信息
```
GET https://api.bimface.com/bdfs/data/v1/projects/{projectId}/fileItems/files
```

### 说明
根据文件项ID或文件项所在路径获取该文件项下所有版本文件的信息。

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
|fileItemId *|文件项ID(fileItemId和path，必须二选一填入）|string|
|path *|文件项所在路径，使用URL编码（UTF-8），最多256个字符（fileItemId和path，必须二选一填入）|string|
|pageNo|开始页码（默认值:1）|int32|
|pageSize|每页大小（默认值:20）|int32|

*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«PagedList«FileItemDTOV1»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|PagedList«FileItemDTOV1»|
|list|查询结果列表|< FileItemDTOV1 >array|
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
https://api.bimface.com/bdfs/data/v1/projects/10000022120160/fileItems/files?fileItemId=10000022200501
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
    "list" : [ {
      "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
      "createTime" : "2023-03-15 15:16:18",
      "id": "10000022200514",
      "fileId": "10000022200514",
      "fileItemId": "10000022200501",
      "name": "BIMFACE示例文件.rvt",
      "fileItemName": "sample.rvt",
      "suffix": "rvt",
      "length": 6787072,
      "projectId": "10000022120160",
      "parentId": "10000022200497",
      "folder": false,
      "storeId": "10000022200512",
      "version": 3,
      "status": "SUCCESS",
      "uploadMode": "DIRECT",
      "md5": "2ACC5AC37B6186C94E49F8EC3D7F971B",
      "physicalIndex": "19f974c24d4b4749be0b7da6a6ae3e37",
      "updateTime": "2023-03-15 15:16:18",
      "current": true,
      "originalCreateTime": "2023-03-08 11:08:50"
    },{
      "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
      "createTime": "2023-03-15 13:18:47",
      "id": "10000022200507",
      "fileId": "10000022200507",
      "fileItemId": "10000022200501",
      "name": "BIMFACE示例模型+标注.rvt",
      "fileItemName": "sample.rvt",
      "suffix": "rvt",
      "length": 6787072,
      "projectId": "10000022120160",
      "parentId": "10000022200497",
      "folder": false,
      "storeId": "10000022200505",
      "version": 2,
      "status": "SUCCESS",
      "uploadMode": "DIRECT",
      "md5": "E9884D857B232188C3D91964352C0FEA",
      "physicalIndex": "70d3649d56d8449a80b5bacdba982b00",    
      "updateTime": "2023-03-15 13:18:47",
      "current": false,
      "originalCreateTime": "2023-03-08 11:08:50"
    },{
      "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
      "createTime": "2023-03-14 15:22:30",
      "id": "10000022200501",
      "fileId": "10000022200501",
      "fileItemId": "10000022200501",
      "name": "Revit示例模型.rvt",
      "fileItemName": "sample.rvt",
      "suffix": "rvt",
      "length": 14450688,
      "projectId": "10000022120160",
      "parentId": "10000022200497",
      "folder": false,
      "storeId": "10000022200499",
      "version": 1,
      "status": "SUCCESS",
      "uploadMode": "DIRECT",
      "md5": "254EB810813A153BE6EBD6ADDD40B75A",
      "physicalIndex": "5b72bc322d874f65a8d8670451063465",
      "updateTime": "2023-03-15 13:13:51",
      "current": false,
      "originalCreateTime": "2023-03-08 11:08:50" 
    }]
}}
```


