# 获取文件夹下的所有文件
```
POST https://api.bimface.com/bdfs/data/v1/projects/{projectId}/folders/contents
```

### 说明
通过接口获取指定文件夹下所有的文件信息，这里的文件指的各个文件项的当前版本，支持根据父文件夹ID或父文件夹路径两种方式获取。


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
|parentId *|父文件夹ID（parentId和parentPath，必须二选一填入）|string|
|parentPath *|父文件夹路径（parentId和parentPath，必须二选一填入）|string|
|fileItemName|文件项名称|string|
|sourceId|调用方文件源ID|string|
|withItemSource|是否返回ItemSource信息，默认为false|boolean|
|pageNo|开始页码，默认为1|int32|
|pageSize|每页大小，默认20|int32|
|startTime|开始时间，格式为YYYY-MM-DD HH:mm:ss|string|
|endTime|结束时间，格式为YYYY-MM-DD HH:mm:ss|string|
|suffix|文件后缀|string|
|useFuzzySearch|当传了name,是否开启模糊查询,默认开启|boolean|
|excludeFolder|是否排除文件夹，默认为true|boolean|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|RestResponse«PagedList«FileItemDTO»»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|PagedList«FileItemDTO»|
|list|查询结果列表|< FileItemDTO >array|
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
https://api.bimface.com/bdfs/data/v1/projects/10000000006016/folders/contents
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "endTime" : "2022-02-04 02:02:02",
  "excludeFolder" : false,
  "parentId" : "10000022200497",
  "startTime" : "2022-02-01 02:02:02",
  "suffix" : "rvt",
  "useFuzzySearch" : false
}
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "data" : {
  "list":[{
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2023-02-23 15:15:13",
    "id": "1938888813662976",
    "fileId": "1938888813662976",
    "fileItemId" : "1938888813661468",
    "name" : "BIMFACE文件第2版本.rvt",
    "fileItemName": "BIMFACE文件.rvt",
    "suffix": "rvt",
    "length": 33411072,
    "projectId" : "10000000006016",
    "parentId" : "10000022200497",
    "folder": false,
    "storeId": "10000022200558",
    "version": 2,
    "status": "SUCCESS",
    "uploadMode": "DIRECT",
    "md5": "8AE732350E05A6DF36F6EF304F03492D",
    "physicalIndex": "a9fa38ecd73d4f7690b28ab3b2adc5e0",
    "updateTime" : "2023-02-23 15:15:13",
    "current": true,
    "originalCreateTime": "2022-02-23 15:14:57"
  },
  {
    "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
    "createTime" : "2023-02-23 15:13:41",
    "id": "1938888813662988",
    "fileId": "1938888813662988",
    "fileItemId" : "1938888813662955",
    "name": "Revit示例模型(流式加载V3.0).rvt",
    "fileItemName": "Revit示例模型.rvt",
    "suffix": "rvt",
    "length": 6459392,
    "projectId" : "10000000006016",
    "parentId" : "10000022200497",
    "folder": false,
    "storeId": "10000022200519",
    "version": 4,
    "status": "SUCCESS",
    "uploadMode": "DIRECT",
    "md5": "AF02C0906F369AE2029EF04AFDB1913F",
    "physicalIndex": "9dedf8734c924b4a8c7c93442ce36e27",
    "updateTime" : "2023-02-23 15:13:41",
    "current": true,
    "originalCreateTime": "2023-02-23 15:13:27"
  }]
  },
  "message" : "success"
}

```


