# 移动文件位置
```
PATCH https://api.bimface.com/bdfs/data/v1/projects/{projectId}/fileItems/moveItem
```
### 说明
移动文件项（包括文件项下所有版本）所在的位置，可以通过文件项ID或文件项所在路径参数指定新的位置，支持批量移动，无法跨项目移动。

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
|fileItemIds *|需要复制的文件项ID集合(fileItemIds和fileItemPath，必须二选一填入)|< string >array|
|fileItemPaths *|需要复制的文件项路径集合(fileItemIds和fileItemPath，必须二选一填入)|< string >array|
|targetParentId *|目标位置上层文件夹ID,(targetParentId和targetParentPath，必须二选一填入)|string|
|targetParentPath *|目标位置上层文件夹路径(targetParentId和targetParentPath，必须二选一填入)|string|

*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|RestResponse«List«FileItemDTO»»|
|**204**|No Content|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< FileItemDTO >array|
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
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/fileItems/moveItem
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "fileItemIds" : [ "1938888813662976", "1938888813662955"],
  "targetParentId" : "10000012360005"
}
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "data" : [ 
{
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2023-02-27 17:18:21",
    "id": "10000022320465",
    "fileId": "10000022320465",
    "fileItemId" : "1938888813662976",
    "name": "附件4.dwg",
    "fileItemName": "图纸对比A26.dwg",
    "suffix": "dwg",
    "length": 64545,
    "projectId" : "10000000006016",
    "parentId" : "10000012360005",
    "folder": false,
    "storeId": "10000022320463",
    "version": 2,
    "status": "SUCCESS",
    "uploadMode": "DIRECT",
    "md5": "5D43B936902C60F464B048CFFCF173B6",
    "physicalIndex": "3a1c5c0c119a43ab910fdf0eb67322a0",
    "updateTime" : "2023-02-27 17:22:32",
    "current": true,
    "originalCreateTime": "2023-02-27 17:18:14"
  },
  {
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2023-02-27 16:35:26",
    "id": "10000022320357",
    "fileId": "10000022320357",
    "fileItemId" : "1938888813662955",
    "name": "F1-风管+桥架 .rvt",
    "fileItemName": "Revit示例模型.rvt",
    "suffix": "rvt",
    "length": 40595456,
    "projectId" : "10000000006016",
    "parentId" : "10000012360005",
    "folder": false,
    "storeId": "10000022320355",
    "version": 2,
    "status": "SUCCESS",
    "uploadMode": "DIRECT",
    "md5": "D3020F205910B25D61D6ACCBF177878A",
    "physicalIndex": "d4c9a255fbd5409f95bf14bb1d044283",   
    "updateTime" : "2023-02-27 17:45:03",
    "current": true,
    "originalCreateTime": "2023-02-24 14:55:38"    
  } ],
  "message" : "success"
}
```


