# 获取构件属性
```
GET https://api.bimface.com/data/v2/integrations/{integrateId}/elements/{elementId}
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
|integrateId *|集成模型ID|integer (int64)|
|elementId *|构件ID|string|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«Property»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|Property|
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

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/integrations/1738888866720224/elements/313052
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
    "boundingBox" : {
      "max" : {
        "x" : -4938.068482562385,
        "y" : -3201.59397858169,
        "z" : 0.0
      },
      "min" : {
        "x" : -4938.068482562385,
        "y" : -3201.59397858169,
        "z" : 0.0
      }
    },
    "elementId" : "313052",
    "familyGuid" : "000222",
    "guid" : "79d547c1-5dbf-4e6a-811d-951cf37b29da-0004c6dc",
    "name" : "norm - 150mm",
    "properties" : [ {
      "group" : "dimension",
      "items" : [ {
        "code" : "perimeter",
        "extension" : "object",
        "key" : "perimeter",
        "orderNumber" : 0,
        "unit" : "mm",
        "value" : 17200,
        "valueType" : 2
      } ]
    } ]
  },
  "message" : null
}
```
