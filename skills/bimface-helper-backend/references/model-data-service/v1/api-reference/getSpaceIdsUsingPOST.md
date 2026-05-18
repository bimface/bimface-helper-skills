# 查询模型包含的空间ID列表
```
POST https://api.bimface.com/data/v1/feature-management/spaces/id-list
```

### 说明

查询模型中包含的空间id列表（含模型解析的空间信息和自定义空间信息）。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|integrateId *|集成ID（fileId和integrateId选填一项）|int64|
|fileId *|文件ID（fileId和integrateId选填一项）|int64|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponseV1«List«EssentialSpaceQueryResponse»»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "code"|string|
|data|返回数据|< EssentialSpaceQueryResponse >array|
|levelId|楼层id|string|
|space|空间信息|< EssentialSpaceBaseInfo >array|
|spaceId|空间id|string|
|name|空间名称|string|
|description|空间描述|string|
|message|提示信息|string|

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v1/feature-management/spaces/id-list
```


##### 请求 header
```json
"Authorization: Bearer dc671840-bacc-4dc5-a134-97c1918d664b"
```


##### 请求 body
```json
{
  "integrateId":"2544652236073248"
}
```


### HTTP响应示例

##### 响应 200
```json
{
    "code": "bimfaceservice-0000",
    "message": null,
    "data": [
        {
            "levelId": "7",
            "space": [
                {
                    "description": null,
                    "name": "房间 1",
                    "spaceId": "10000779773337_1785883"
                }
            ]
        }
    ]
}
```