# 获取转换资源列表
```
POST https://api.bimface.com/v1/{projectId}/translations
```

### 说明
应用发起转换以后，可以根据筛选条件，通过该接口批量查询指定项目下的转换详情列表，返回值可包含文件夹层级关系。(该接口仅支持公有云环境应用。)

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|projectId *|项目ID|integer (int64)|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|startDate|发起转换开始日期，采用ISO8601标准表示，并使用UTC +0时间，格式为yyyy-MM-ddTHH:mm:ss.MSZ|date-time|
|endDate|发起转换结束日期，采用ISO8601标准表示，并使用UTC +0时间，格式为yyyy-MM-ddTHH:mm:ss.MSZ|date-time|
|pageSize|每页记录条数，默认为20，最大不可超过1000|int32|
|suffix|后缀|string|
|keyword|关键字，可查询ID或名称|string|
|recursive|递归，true表示能查询当前及以下文件夹下的文件，false表示能查询当前文件夹下的文件（不包括子文件夹），默认为false|boolean|
|outputFormat|输出格式|string|
|parentId|父目录文件夹ID|string|
|excludeFolder|是否排除文件夹，默认为false|boolean|
|status|转换状态，99代表转换成功，-1代表转换失败，1代表转换中，-2代表支付失败，0代表准备中|int32|
|searchAfter|分页查询条件，可基于上一页的查询结果帮助检索下一页|array|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|OpenResponse«PagedList«FileTranslateResponse»»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|PagedList«FileTranslateResponse»|
|pageSize|每页记录数|int32|
|list|查询结果列表|< FileTranslateResponse >array|
|sourceId|资源ID|string|
|databagId|数据包ID|string|
|isFolder|是否为文件夹|boolean|
|reason|失败原因|string|
|progressPercent|转换进度|int32|
|cost|转换运行时间|int32|
|length|文件大小|int64|
|errorCode|错误代码|string|
|shareToken|分享Token|string|
|offlineDatabagStatus|离线数据包状态|string|
|createTime|转换创建时间，采用ISO8601标准表示，并使用UTC +0时间，格式为yyyy-MM-ddTHH:mm:ss.MSZ|string|
|name|文件名|string|
|realFileType|源文件类型|string|
|appKey|appKey|string|
|shareUrl|分享链接|string|
|outputFormat|输出格式|string|
|projectId|项目ID|int64|
|fileId|文件ID|int64|
|status|文件转换状态|string|
|searchAfter|本条返回数据之后的分页查询条件，若为null，则表示所有数据均返回，后续无其他数据。|array|
|message|提示信息|string|

### 消耗

* `application/json`

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/v1/10000000006016/translations
```

##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

##### 请求 body
```json
{
  "keyword" : "BIMFACE",
  "outputFormat" : "bimtiles",
  "pageSize" : 100,
  "parentId" : "10000000486016",
  "status" : 99
}
```

### HTTP响应示例

##### 响应 200
```json
{
  "code": "bimfaceservice-0000",
  "message": null,
  "data": {
    "pageSize":100,
    "list": [
      {
        "appKey": "odatvZYUSAWMbdUjTU8HoZXB9tFt6123",
        "cost": 349,
        "createTime": "2022-01-01T07:00:00.000Z",
        "databagId": "6f34dd8678c65f6f5da5cee04e964718",
        "errorCode": null,
        "fileId": 1938888813662976,
        "length": 6459392,
        "name": "BIMFACE示例模型.rvt",
        "offlineDatabagStatus": "success",
        "outputFormat": "bimtiles",
        "projectId": "10000000006016",
        "realFileType": "rvt",
        "reason": null,
        "shareToken": null,
        "shareUrl": null,
        "sourceId": null,
        "status": "success",
        "progressPercent": 100
      }
    ],
    "searchAfter": null
  }
}
```

说明：当返回值超出所设置的pageSize时，需通过返回的searchAfter值进行多轮查询。例如请求待获取的资源共30条数据，且设置了每页记录条数为20。为获取所有30条数据，需发送两次请求:

1.第一次请求
获取前20条数据，并拿到第二次查询所需的searchAfter值。

（1）请求body
```json
{
  "pageSize" : 20,
  "parentId" : "10000000486016"
}
```

（2）响应返回值
```json
{
  "code": "bimfaceservice-0000",
  "message": null,
  "data": {
    "pageSize":20,
    "list": [
      // 返回前20条数据 
    ],
    "searchAfter": [
      0,
      1647284533000,
      1647278182000,
      "rlm2h38BUCkEvVdO0bim"
    ]
  }
}
```

2.第二次请求
获取后10条数据，基于第一次查询获取的searchAfter值进行请求，当返回值内searchAfter值为null时，代表所有数据已返回，后续无其他数据。

（1）请求body
```json
{
  "pageSize" : 20,
  "parentId" : "10000000486016",
  "searchAfter" : [
    0,
    1647284533000,
    1647278182000,
    "rlm2h38BUCkEvVdO0bim"
  ]
}
```

（2）响应返回值
```json
{
  "code": "bimfaceservice-0000",
  "message": null,
  "data": {
    "pageSize":20,
    "list": [
      // 返回后10条数据 
    ],
    "searchAfter": null
  }
}
```
