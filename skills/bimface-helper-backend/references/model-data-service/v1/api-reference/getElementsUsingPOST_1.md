# 批量获取构件属性
```
POST https://api.bimface.com/data/v2/integrations/{integrateId}/elements
```

### 说明
在请求体里可以传入多个指定子文件/链接内的构件ID列表来批量获取属性，单次请求的构件数量上限为1000。若需根据修改后的属性值查询，需要设置请求参数includeOverrides的值为true。


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
|includeOverrides|是否查询修改的属性|boolean|

##### Body
|名称|说明|类型|
|---|---|---|
|filter *|筛选条件|< PropertyFilterGroupAndKeysPair >array|
|  keys|属性字段|< string >array|
|  group|分组类型|string|
|ids *|由文件ID及构件ID构成对象组成的列表|< FileIdHashWithElementIds >array|
|  fileIdHash|构件所属的模型ID|string|
|  elementIds|构件ID列表|< object >array|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«Property»»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< Property >array|
|elementId|构件ID|string|
|boundingBox|包围盒信息|BoundingBox|
|min|最小坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|max|最大坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|name|构件族名称|string|
|guid|全局唯一标识符|string|
|familyGuid|族标识符|string|
|properties|构件属性|< PropertyGroup >array|
|items|属性数据|< PropertyItem >array|
|extension|拓展属性|object|
|unit|单位|string|
|code|属性码|string|
|orderNumber||int32|
|valueType|参数值类型|int32|
|value|参数值|object|
|key|特性|string|
|group|属性分组类型|string|
|message|提示消息|string|

### 消耗

* `application/json`

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/integrations/1738888866720224/elements
```

##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

##### 请求 body
```json
{
  "filter" : [ {
    "group" : "基本属性",
    "keys" : [ "family" ]
  } ],
  "ids" : [ {
    "fileIdHash" : "1938263713662976",
    "elementIds" : [ "218286", "110930" ]
  } ]
}
```

### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "data" : [ {
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
    "elementId" : "218286",
    "familyGuid" : "-1",
    "guid" : "218286",
    "name" : "norm - 150mm",
    "properties" : [ {
      "group" : "基本属性",
      "items" : [ {
        "key" : "family",
        "value" : "基本屋顶"
      } ]
    } ]
  },{
      "boundingBox": {
      "max": {
        "x": 3120.64,
        "y": 2008.61,
        "z": 720.11
      },
      "min": {
        "x": -1674.38,
        "y": -4060.52,
        "z": 0
      }
    },
    "elementId" : "110930",
    "familyGuid" : "-1",
    "guid" : "110930",
    "name" : "norm - 400mm",
    "properties" : [ {
      "group" : "基本属性",
      "items" : [ {
        "key" : "family",
        "value" : "基本屋顶"
      } ]
    } ]
  } ],
  "message" : null
}
```
