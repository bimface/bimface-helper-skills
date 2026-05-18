# 创建追加文件
```
POST https://api.bimface.com/bdfs/data/v1/projects/{projectId}/fileItems/appendFile
```
### 说明
当文件比较大或者所在网络网速较慢的情况下，单个文件上传的时间会比较长。如果中间有任何的网络错误， 本次上传就会被中断，文件重新上传将浪费大量时间。

BIMFACE提供了断点上传的接口，即使上传过程中出现错误， 下一次也可以从上次错误的文件位置开始继续上传。

进行断点续传前，先申请创建一个追加文件，并记录该追加文件ID。

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
|name *|文件名称|string|
|length *|文件流的长度|int64|
|parentId *|父文件夹ID（parentId和parentPath，必须二选一填入）|string|
|parentPath *|父文件夹路径（parentId和parentPath，必须二选一填入）|string|
|sourceId|调用方的文件源ID，不能重复|string|

*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|RestResponse«FileItemAppendFileDTO»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|FileItemAppendFileDTO|
|sourceId|调用方的文件源ID|string|
|type|AppendFile（追加上传文件）的类型，分为【FILEITEM、OBJECTITEM】|string|
|name|文件名称|string|
|id|AppendFile（追加上传文件）关联的ID|string|
|position|上传的位置|int64|
|projectId|项目ID|string|
|fileItemDTO|AppendFile关联的具体文件信息|FileItemDTO|
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
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/fileItems/appendFile?length=345345345&name=sample.rvt
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
    "fileItemDTO" : {
      "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
      "createTime" : "2022-02-02 02:02:02",
      "id": "1938888813662976",
      "fileId": "1938888813662976",
      "fileItemId" : "1938888813662976",
      "name": "sample.rvt",
      "fileItemName": "sample.rvt",
      "suffix" : "rvt",
      "length" : 345345345,
      "projectId" : "10000000006016",
      "parentId" : "10000000006016",
      "folder" : false,
      "storeId" : "10000000021156",
      "version" : 1,    
      "status" : "success",
      "uploadMode" : "GENERAL",
      "md5" : "sdfhskbvnksdiuewriusbndskudf",
      "physicalIndex" : "a72eaf22f4214a6384429f78b690c983",
      "updateTime" : "2022-02-02 02:02:02",
      "current": true,
      "originalCreateTime": "2022-02-02 02:02:02"
    },
    "id" : "1938888813662976",
    "name" : "sample.rvt",
    "position" : 0,
    "projectId" : "10000000006016",
    "sourceId" : "bimfaceTestSourceId",
    "type" : "FILEITEM"
  },
  "message" : "success"
}
```


