# 批量获取转换状态详情
```
POST https://api.bimface.com/translateDetails
```

### 说明
应用发起转换以后，可以根据筛选条件，通过该接口批量查询转换状态详情

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|sourceId|资源ID|string|
|fileName|文件名|string|
|endDate|结束日期|date-time|
|pageSize|每页记录条数|int32|
|suffix|后缀|string|
|sortType|样例: "sortType"|string|
|pageNo|页码|int32|
|appKey|appKey|string|
|keyword|关键字|string|
|outputFormat|输出格式|string|
|projectId *|项目ID|int64|
|startDate|开始日期|date-time|
|fileId|文件ID|int64|
|status|转换状态，99代表转换成功，-1代表转换失败，1代表转换中，-2代表支付失败，0代表准备中|int32|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«PagedList«FileTranslateDetailBean»»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|PagedList«FileTranslateDetailBean»|
|page||Page|
|startIndex||int32|
|prePage|上一页|int32|
|nextPage|下一页|int32|
|pageNo|页码|int32|
|htmlDisplay||string|
|totalPages|总页数|int32|
|pageSize|每页记录条数|int32|
|totalCount|总条数|int32|
|list||< FileTranslateDetailBean >array|
|sourceId|资源ID|string|
|databagId|数据包ID|string|
|reason|样例: "reason"|string|
|thumbnail|缩略图|< string >array|
|cost|运行时间|int32|
|length|文件大小|int64|
|errorCode|错误代码|string|
|shareToken|分享Token|string|
|priority|优先级|int32|
|type|文件类型|string|
|offlineDatabagStatus|离线数据包状态|string|
|supportOfflineDatabag||boolean|
|createTime|创建时间|string|
|name|文件名|string|
|realFileType|样例: "realFileType"|string|
|appKey|样例: "appKey"|string|
|shareUrl|分享链接|string|
|outputFormat|输出格式|string|
|projectId|项目ID|int64|
|retry||boolean|
|fileId|文件ID|int64|
|status|文件状态|string|
|progressPercent|转换进度|int32|
|message|提示信息|string|

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/translateDetails
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "appKey" : "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
  "endDate" : "string",
  "fileId" : 1938888813662976,
  "fileName" : "fileName",
  "keyword" : "keyword",
  "outputFormat" : "outputFormat",
  "pageNo" : 0,
  "pageSize" : 0,
  "projectId" : 10000000006016,
  "sortType" : "sortType",
  "sourceId" : "d4649ee227e345c8b7f0022342247dec",
  "startDate" : "string",
  "status" : 0,
  "suffix" : "suffix"
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
                "cost": 349,
                "createTime": "2021-08-09 17:24:35",
                "databagId": "6f34dd8678c65f6f5da5cee04e964718",
                "errorCode": null,
                "fileId": 1938888813662976,
                "length": 6459392,
                "name": "BIMFACE示例模型.rvt",
                "offlineDatabagStatus": "success",
                "outputFormat": "bmd",
                "priority": null,
                "projectId": "10000000006016",
                "realFileType": "rvt",
                "reason": null,
                "retry": true,
                "shareToken": null,
                "shareUrl": null,
                "sourceId": null,
                "status": "success",
                "progressPercent": 100,
                "supportOfflineDatabag": true,
                "thumbnail": null,
                "type": "rvt-translate"
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


