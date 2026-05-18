# 发起碰撞检测
```
POST https://api.bimface.com/clash-detective/v1/clash-detective
```


### 说明
发起多模型碰撞检测，碰撞检测可分为以下两类：

（1）硬碰撞(Hard)：指两个构件在空间上发生碰撞（正好接触的构件不会视为碰撞），如管道穿过结构框架柱；

（2）间隙碰撞(Clearance)：两个构件在空间上并未发生实际碰撞，但构件之间预留空间过小，预留距离无法满足施工工艺或设备安装需求。


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|selectionA *|选择集A|< ClashDetectiveSource >array|
|  fileId|文件ID（fileId和integrateId选填一项）|int64|
|  integrateId|集成ID（fileId和integrateId选填一项）|int64|
|  objectData|构件筛选条件，筛选字段可通过getObjectDataById方法获取（objectData和objectIds至少填一项）|< Map«string,string» >array|
|  objectIds|构件ID的数组（objectData和objectIds至少填一项）|< string >array|
|  transformation|模型转换矩阵|< string >array|
|selectionB *|选择集B|< ClashDetectiveSource >array|
|  fileId|文件ID（fileId和integrateId选填一项）|int64|
|  integrateId|集成ID（fileId和integrateId选填一项）|int64|
|  objectData|构件筛选条件，筛选字段可通过getObjectDataById方法获取（objectData和objectIds至少填一项）|< Map«string,string» >array|
|  objectIds|构件ID的数组（objectData和objectIds至少填一项）|< string >array|
|  transformation|模型转换矩阵|< string >array|
|clashType *|碰撞类型，"Hard"为硬碰撞，"Clearance"为间隙碰撞|string|
|tolerance|碰撞公差，单位为mm（精确到0.1），仅间隙碰撞支持设置公差，当间隙小于等于公差时视为碰撞，碰撞类型为"Clearance"时，本参数为必填项|number(double)|
|config|配置参数,非必填|object|
|  selfIntersect|配置需要自碰撞的选择集, 默认均不做自碰撞；示例：["selectionA","selectionB"]|< string >array|
|callback|回调url|string|

### 响应

| HTTP代码 | 说明         | 类型                                    |
| -------- | ------------ | --------------------------------------- |
| **200**  | OK           | GeneralResponse«ClashDetectiveResponse» |
| **201**  | Created      | -                                       |
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

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/clash-detective/v1/clash-detective
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "callback": "",
  "clashType": "Hard",
  "selectionA":[
    {
        "fileId": 1938888813662976,
        "objectData": [{
        "categoryId": "-2001330"
        }],
        "objectIds": ["272828","4567897"]
    },
    {
        "fileId": 22222000000000,
        "objectData": [{
        "categoryId": "-2000001"
        }]
    }
  ],
  "selectionB": [
    {
        "fileId": 19387777777777,
        "objectData": [{
        "family": "混凝土-矩形梁-C30"
        }]
    }
  ]
}
```

### HTTP响应示例

##### 响应 200
```json
{
  "code": "bimfaceservice-0000",
  "message": null,
  "data": {
    "clashDetectiveId": 2265157542875776,
    "createTime": "2023-01-05T07:39:17.000Z",
    "databagVersion": "3.0",
    "errorCode": null,
    "projectId": "10000000006016",
    "reason": null,
    "selectionA": [
            {
                "fileId": 1938888813662976,
                "integrateId": null
            },
            {
                "fileId": 22222000000000,
                "integrateId": null
            }
        ],
        "selectionB": [
            {
                "fileId": 19387777777777,
                "integrateId": null
            }
        ],
    "status": "processing"
  }
}
```


