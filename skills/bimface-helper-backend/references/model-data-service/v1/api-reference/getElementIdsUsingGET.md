# 查询满足条件的构件
```
GET https://api.bimface.com/data/v2/files/{fileId}/elementIds
```

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|fileId *|文件ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|categoryId|构件ID|string|
|family|族名称|string|
|familyType|族类型|string|
|floor|楼层|string|
|paginationContextId|根据paginationContextId返回构件ID列表|string|
|paginationSize|返回结果按照paginationSize分页|integer (int32)|
|roomBoundaryExtensionXY|房间在平面范围的边界扩展值，不小于0|number (double)|
|roomBoundaryExtensionZ|房间在高度范围的边界扩展值，不小于0|number (double)|
|spaceId|空间ID或者房间ID,兼容roomId的值|string|
|roomToleranceXY|XY坐标轴方向对构件的筛选容忍度|enum (STRICT, ORDINARY, LENIENT)|
|roomToleranceZ|Z坐标轴方向对构件的筛选容忍度|enum (STRICT, ORDINARY, LENIENT)|
|specialty|专业|string|
|systemType|系统类型|string|
|excludeInvisibles|是否剔除无几何信息的构件，默认为false|boolean|

>说明：roomToleranceXY及roomToleranceZ为构件坐标与room坐标的包含关系，有STRICT,ORDINARY,LENIENT三种状态，具体说明可见[构件空间关系计算](https://bimface.com/docs/model-data-service/v1/developers-guide/spatial-relationship.html)。

### 响应

| HTTP代码 | 说明         | 类型                              |
| -------- | ------------ | --------------------------------- |
| **200**  | OK           | SingleModelElementsSwaggerDisplay |
| **401**  | Unauthorized | -                                 |
| **403**  | Forbidden    | -                                 |
| **404**  | Not Found    | -                                 |

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/files/1938888813662976/elementIds
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
    "data": [
        "272385",
        "393020"
    ]
}
```
