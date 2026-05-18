# 查询满足条件的构件
```
GET https://api.bimface.com/data/v2/integrations/{integrateId}/elementIds
```

### 说明
根据六个维度（专业，系统类型，楼层，构件类型，族，族类型）获取对应的构件ID列表，任何维度都是可选的。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|Bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|integrateId *|集成模型ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|categoryId|筛选条件：构件类型id|string|
|family|筛选条件：族|string|
|familyType|筛选条件：族类型|string|
|floor|筛选条件：楼层|string|
|paginationContextId|根据paginationContextId返回构件ID列表|string|
|paginationSize|返回结果按照paginationSize分页|integer (int32)|
|spaceId|空间ID或者房间ID,兼容roomId的值|string|
|roomToleranceXY|XY坐标轴方向对构件的筛选容忍度|enum (STRICT, ORDINARY, LENIENT)|
|roomToleranceZ|Z坐标轴方向对构件的筛选容忍度|enum (STRICT, ORDINARY, LENIENT)|
|specialty|筛选条件：专业|string|
|systemType|筛选条件：系统类型|string|
|excludeInvisibles|是否剔除无几何信息的构件，默认为false|boolean|

>说明：roomToleranceXY及roomToleranceZ为构件坐标与room坐标的包含关系，有STRICT,ORDINARY,LENIENT三种状态，具体说明可见[构件空间关系计算](https://bimface.com/docs/model-data-service/v1/developers-guide/spatial-relationship.html)。

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«ElementsWithBoundingBox»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|ElementsWithBoundingBox|
|boundingBox|包围盒信息|BoundingBox|
|min|最小坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|max|最大坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|elements|构件数据列表|< ElementIdWithFileId >array|
|elementId|构件ID|string|
|fileId|构件所属的模型ID|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/integrations/1738888866720224/elementIds
```

##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "data" : {
    "boundingBox": {
      "max": {
        "x": 3131.64,
        "y": 3008.61,
        "z": 1720.11
      },
      "min": {
        "x": -2674.38,
        "y": -5160.52,
        "z": 0
      }
    },
    "elements" : [ {
      "elementId" : "281218",
      "fileId" : "1938888813662976"
    },
    {
      "elementId" : "281219",
      "fileId" : "1938888813662976"
    },
    {
      "elementId" : "281220",
      "fileId" : "1938888813662976"
    },
    {
      "elementId" : "300042",
      "fileId" : "1938888813662976"
    }]
  },
  "message" : null
}
```
