# 查询碰撞检测状态
```
GET https://api.bimface.com/clash-detective/v1/clash-detective-state
```

### 说明
发起碰撞检测后，可以通过该接口查询碰撞检测的状态


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|clashDetectiveId *|碰撞检测ID|integer (int64)|
*为必填项

### 响应

| HTTP代码 | 说明         | 类型                                    |
| -------- | ------------ | --------------------------------------- |
| **200**  | OK           | GeneralResponse«ClashDetectiveResponse» |
| **401**  | Unauthorized | -                                       |
| **403**  | Forbidden    | -                                       |
| **404**  | Not Found    | -                                       |


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|ClashDetectiveResponse|
|reason|失败原因|string|
|clashDetectiveId|碰撞检测ID|int64|
|selectionA|选择集A中参与碰撞的文件ID|Array|
|selectionB|选择集B中参与碰撞的文件ID|Array|
|createTime|创建时间|string|
|errorCode|错误码|string|
|databagVersion|数据包版本|string|
|status|任务状态|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/clash-detective/v1/clash-detective-state?clashDetectiveId=2265157542875776
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


### HTTP响应示例

##### 响应 200
```json
{   
   "code":  "bimfaceservice-0000",   
  "message": null,
  "data": {
    "clashDetectiveId": 2265157542875776,
    "selectionA":[
            {
                "fileId":"1938888813660001",
                "integrateId": null
            },
            {
                "fileId":"1938888813660002",
                "integrateId": null
            }
        ],
    "selectionB":[
            {
                "fileId":null,
                "integrateId":"1938888813660004"
            },
            {
                "fileId":"1938888813660005",
                "integrateId": null
            }
        ],
    "createTime": "2023-01-05T07:39:17.000Z",
    "databagVersion": "3.0",
    "errorCode": null,
    "projectId": "10000000006016",
    "reason": null,
    "status": "processing"
}
}
```


