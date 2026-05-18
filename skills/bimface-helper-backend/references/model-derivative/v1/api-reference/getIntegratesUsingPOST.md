# 批量查询集成状态
```
POST https://api.bimface.com/integrateDetails
```

### 说明
调用方发起集成以后，可以根据筛选条件，通过该接口批量查询集成模型状态详情

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|integrateId|集成ID|int64|
|sourceId|资源ID|string|
|fileName|文件名|string|
|endDate|结束日期|date-time|
|pageSize|每页记录条数|int32|
|integrateType|集成类型|string|
|sortType|样例: "sortType"|string|
|pageNo|页码|int32|
|appKey|appkey|string|
|keyword|关键字|string|
|outputFormat|输出格式|string|
|projectId *|项目ID|int64|
|startDate|开始日期|date-time|
|status|任务状态，99代表集成成功，-1代表集成失败，1代表集成中，-2代表支付失败，0代表准备中|int32|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«PagedList«FileIntegrateDetailBean»»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|PagedList«FileIntegrateDetailBean»|
|page|分页信息|Page|
|startIndex||int32|
|prePage|上一页|int32|
|nextPage|下一页|int32|
|pageNo|页码|int32|
|htmlDisplay||string|
|totalPages|总页数|int32|
|pageSize|每页记录条数|int32|
|totalCount|总条数|int32|
|list||< FileIntegrateDetailBean >array|
|integrateId|集成ID|int64|
|sourceId|资源ID|string|
|workerType|worker类型|string|
|databagId|数据包ID|string|
|reason|失败原因|string|
|thumbnail|缩略图|< string >array|
|cost|处理时间|int32|
|errorCode|错误代码|string|
|shareToken|分享token|string|
|priority|优先级|int32|
|type|模型格式|string|
|offlineDatabagStatus|离线数据包状态|string|
|createTime|创建时间|string|
|name|集成模型名称|string|
|appKey||string|
|shareUrl|分享链接|string|
|outputFormat|输出格式|string|
|projectId|项目ID|int64|
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
https://api.bimface.com/integrateDetails
```


##### 请求 header
```json
"Authorization: Bearer dc671840-bacc-4dc5-a134-97c1918d664b"
```


##### 请求 body
```json
{
  "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
  "endDate" : "2021-09-30",
  "fileName" : "fileName",
  "integrateId" : 0,
  "integrateType" : "integrateType",
  "keyword" : "keyword",
  "outputFormat" : "bimtiles",
  "pageNo" : 0,
  "pageSize" : 0,
  "projectId" : 0,
  "sortType" : "sortType",
  "sourceId" : "cf23fe0611b84d06979a1a9395b273fc",
  "startDate" : "string",
  "status" : 99
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
                "appKey": "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
                "cost": 66,
                "createTime": "2021-10-27 16:18:12",
                "databagId": "6f34dd8678c65f6f5da5cee04e964718",
                "errorCode": null,
                "integrateId": 2241076842735264,
                "name": "集成test",
                "offlineDatabagStatus": "prepare",
                "outputFormat": "bmd",
                "priority": null,
                "projectId": "10000000006016",
                "reason": null,
                "shareToken": null,
                "shareUrl": null,
                "sourceId": null,
                "status": "success",
                "thumbnail": null,
                "type": "rvt",
                "workerType": "rvt"
            }
        ],
        "page": {
            "nextPage": 1,
            "pageNo": 1,
            "pageSize": 20,
            "prePage": 1,
            "startIndex": 0,
            "totalCount": 1,
            "totalPages": 1
        }
    }
}
```


