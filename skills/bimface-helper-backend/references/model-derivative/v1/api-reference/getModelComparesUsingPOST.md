# 批量获取文件对比状态
```
POST https://api.bimface.com/compares
```


### 说明
应用发起对比以后，可以根据筛选条件批量查询对比状态


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|sourceId|调用方的文件源ID|string|
|fileName|文件名称|string|
|endDate|结束日期|date-time|
|pageSize|每页记录条数|int32|
|compareId|对比ID|int64|
|type|文件类型|string|
|sortType|排序方式|string|
|pageNo|页码|int32|
|appKey|appKey|string|
|keyword|关键字|string|
|ignoreOfflineDatabagInfo|是否忽略离线数据包信息|boolean|
|projectId *|项目ID|int64|
|startDate|开始日期|date-time|
|status|任务状态|int32|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«PagedList«ModelCompareBean»»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|返回数据|PagedList«ModelCompareBean»|
|page||Page|
|startIndex|起始页码|int32|
|prePage|上一页|int32|
|nextPage|下一页|int32|
|pageNo|页码|int32|
|totalPages|总页数|int32|
|pageSize|每页记录条数|int32|
|totalCount|总条数|int32|
|list||< ModelCompareBean >array|
|sourceId|调用方的文件源ID|string|
|workerType|worker类型|string|
|reason|失败原因|string|
|thumbnail|缩略图|< object >array|
|cost|耗时|int32|
|errorCode|错误码|string|
|compareId|对比ID|int64|
|priority|优先级|int32|
|type|文件类型|string|
|offlineDatabagStatus|离线数据包状态|string|
|createTime|创建时间|string|
|name|对比名称|string|
|projectId|对比ID|int64|
|status|任务状态|string|
|message|提示消息|string|

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/compares
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "appKey" : "appKey",
  "endDate": "2021-09-27",
  "startDate": "2019-05-01",
  "ignoreOfflineDatabagInfo" : true,
  "pageNo" : 1,
  "pageSize" : 10,
  "projectId" : 10000000006016,
  "status" : "success",
  "type" : "rvt"
}
```


### HTTP响应示例

##### 响应 200
```json
{
  "code": "success",
  "message": null,
  "data": {
    "list": [
      {
        "compareId": 2077707858585728,
        "cost": null,
        "createTime": "2020-06-19 14:13:21",
        "errorCode": null,
        "name": "compare0001",
        "offlineDatabagStatus": "prepare",
        "priority": 2,
        "projectId": "10000000006016",
        "reason": null,
        "sourceId": null,
        "status": "success",
        "thumbnail": null,
        "type": "rvt",
        "workerType": "model-compare"
      }
    ],
    "page": {
      "nextPage": 1,
      "pageNo": 1,
      "pageSize": 10,
      "prePage": 1,
      "startIndex": 0,
      "totalCount": 1,
      "totalPages": 1
    }
  }
}
```


