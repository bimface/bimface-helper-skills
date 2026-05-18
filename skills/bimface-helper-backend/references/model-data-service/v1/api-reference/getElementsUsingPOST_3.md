# 查询符合条件的构件ID列表
```
POST https://api.bimface.com/data/v2/query/elementIds
```

### 说明
查询符合条件的构件ID列表，支持基于DSL进行组合查询。DSL 语法详见 [dsl-query.md](../../../dsl-query.md)，也可参考 BIMFACE [官方文档](https://bimface.com/docs/model-data-service/v1/developers-guide/query-instruction.html)。若需根据修改后的属性值查询，需要设置请求参数includeOverrides的值为true。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|includeOverrides|是否根据修改后的属性值查询，默认为false|boolean|
|excludeInvisibles|是否剔除无几何信息的构件，默认为false|boolean|
|paginationContextId|根据paginationContextId返回构件ID列表|string|
|paginationSize|返回结果按照paginationSize分页|integer (int32)|

##### Body
|名称|说明|类型|
|---|---|---|
|dsl *|查询DSL|string|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«SearchElementIdsResp»»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|返回数据|< SearchElementIdsResp >array|
|targetId|文件ID|string|
|elementIds|构件ID列表|< object >array|
|message|提示消息|string|

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/query/elementIds
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "targetType": "integration",
  "targetIds": [
    "2938888813662976"
  ],
  "query": {
    "boolAnd":[
      {"match":{"fileId":"1648888854324461"}},
      {"contain": {"floor": "F2"}}
    ]
  }
}
```


### HTTP响应示例

##### 响应 200
```json
{
  "code": "success",
  "message": null,
  "data": [
    {
      "elementIds": [
        "1648888854324461.259504",
        "1648888854324461.259778",
        "1648888854324461.260503", 
        "1648888854324461.390503"
      ],
      "targetId": "2938888813662976"
    }
  ]
}
```
