# 根据模型ID查询碰撞检测ID列表
```
GET https://api.bimface.com/clashDetectiveList
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
|code|样例: "success"|string|
|data|返回数据|ClashDetectiveList|
|items|碰撞检测列表|< ClashItemBean >array|
|clashDetectiveId|碰撞检测ID|int64|
|createTime|创建时间|string|
|fileIdA|选择集A的文件ID|int64|
|fileIdB|选择集B的文件ID|int64|
|integrateIdA|选择集A的集成ID|int64|
|integrateIdB|选择集B的集成ID|int64|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/clashDetectiveList?fileId=1938888813662976
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


### HTTP响应示例

##### 响应 200
```json
{
  "code": "success",
  "message": null,
  "data": {
    "items": [
      {
        "clashDetectiveId": 2265157542875776,
        "createTime": "2021-11-30 16:50:30",
        "fileIdA": 1938888813662976,
        "fileIdB": 1938888813662976,
        "integrateIdA": null,
        "integrateIdB": null
      }
    ]
  }
}
```


