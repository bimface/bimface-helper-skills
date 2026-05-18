# 根据模型ID查询碰撞检测ID列表
```
GET https://api.bimface.com/clash-detective/v1/clash-detective-list
```

### 说明
根据模型ID查询碰撞检测ID列表


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|fileId|文件ID（fileId和integrateId选填一项）|integer (int64)|
|integrateId|集成ID（fileId和integrateId选填一项）|integer (int64)|

### 响应

| HTTP代码 | 说明         | 类型                                |
| -------- | ------------ | ----------------------------------- |
| **200**  | OK           | GeneralResponse«ClashDetectiveList» |
| **401**  | Unauthorized | -                                   |
| **403**  | Forbidden    | -                                   |
| **404**  | Not Found    | -                                   |


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|ClashDetectiveList|
|items|碰撞检测列表|< ClashItemBean >array|
|clashDetectiveId|碰撞检测ID|int64|
|createTime|创建时间|string|
|selectionA|选择集A的文件ID|array|
|selectionB|选择集B的文件ID|array|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/clash-detective/v1/clash-detective-list?fileId=1938888813660001
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


### HTTP响应示例

##### 响应 200
```json
{
    "code": "bimfaceservice-0000",
    "message": null,
    "data": {
        "items": [
            {
                "clashDetectiveId": 2614030179855840,
                "createTime": "2023-01-05T07:39:17.000Z",
                 "selectionA":[
            {
                "fileId":"1938888813660001",
                "integrateId": null
            },
            {
                "fileId":"1938888813660002",
                "integrateId": null
            },
            {
                "fileId":null,
                "IntegrateId":"1938888813660003"
            }
        ],
        "selectionB":[
            {
                "fileId":"1938888813660004",
                "integrateId": null
            },
            {
                "fileId":"1938888813660005",
                "integrateId": null
            },
            {
                "fileId":null,
                "IntegrateId":"1938888813660006"
            }
        ]
            },
            {
                "clashDetectiveId": 2614023056149728,
                "createTime": "2023-01-05T07:39:17.000Z",
                "selectionA":[
             {
                "fileId":"1938888813660001",
                "integrateId": null
            },
            {
                "fileId":"1938888813660002",
                "integrateId": null
            },
            {
                "fileId":null,
                "IntegrateId":"1938888813660003"
            }
        ],
        "selectionB":[
            {
                "fileId":"1938888813660004",
                "integrateId": null
            },
            {
                "fileId":"1938888813660005",
                "integrateId": null
            },
            {
                "fileId":null,
                "IntegrateId":"1938888813660006"
            }
        ]
            }
        ]
    }
}
```


