# 获取项目根文件夹信息
```
GET https://api.bimface.com/bdfs/v1/domain/hubs/{hubId}/projects/{projectId}/root-folder
```
### 说明
获取项目下的根文件夹信息，包括根文件夹路径。每个项目拥有对应且唯一的根文件夹，项目下所有文件夹/文件都存放于该根文件夹下。

根文件夹和项目绑定，随项目自动创建、更新、删除，不支持针对根文件夹的移动、复制、删除等操作。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|hubId *|hubId|string|
|projectId *|项目ID|string|

*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«FileItemWithPathDTO»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|FileItemWithPathDTO|
|appKey|appKey|string|
|createTime|文件创建时间|string|
|current|是否为当前版本|boolean|
|fileId|文件ID|string|
|fileItemId|文件项ID|string|
|fileItemName|文件项名称|string|
|folder|是否为文件夹|boolean|
|path|文件夹路径|string|
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
https://api.bimface.com/bdfs/v1/domain/hubs/10000000000060002/projects/10000000006016/root-folder
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "bimfaceservice-0000",
  "data" : {
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2022-02-02 02:02:02",
    "id" : "10000000006016",
    "fileId": "10000000006016",
    "fileItemId" : "10000000006016",
    "name" : "/",
    "fileItemName" : "/",
    "suffix" : null,
    "length" : 0,
    "projectId" : "10000000006016",
    "parentId" : null,
    "folder" : true,
    "storeId" : null,
    "version" : 1,
    "status" : "success",
    "uploadMode" : null,
    "md5" : null,
    "physicalIndex" : null,
    "updateTime" : "2022-02-02 02:02:02",
    "current": true,
    "path" : "/",
    "originalCreateTime": "2022-02-02 02:02:02",
  },
  "message" : null
}
```


