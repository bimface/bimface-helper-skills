# 获取楼层对应面积分区列表
```
GET https://api.bimface.com/data/v2/integrations/{integrateId}/areas
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
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|floorId *|楼层ID|string|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«Area»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< Area >array|
|area|面积|double|
|boundary|边界坐标|string|
|viewName|view名称|string|
|maxPt|最大坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|levelId|area对应的楼层ID|string|
|minPt|最小坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|perimeter|area底部周长|double|
|name|area名称|string|
|id|area的ID|string|
|boundarySegments|附着构件ID|< string >array|
|properties|属性列表|< PropertyGroup >array|
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
https://api.bimface.com/data/v2/integrations/1738888866720224/areas?floorId=1
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
  "data" : [ {
    "area" : 5.16E7,
    "boundary" :{"loops":[[[{"x":-2845.05,"y":5899.48,"z":0.0},{"x":-7745.05,"y":5899.48,"z":0.0}],[{"x":-7745.05,"y":5899.48,"z":0.0},{"x":-7745.05,"y":1399.48,"z":0.0}],[{"x":-7745.05,"y":1399.48,"z":0.0},{"x":-2845.05,"y":1399.48,"z":0.0}],[{"x":-2845.05,"y":1399.48,"z":0.0},{"x":-2845.05,"y":5899.48,"z":0.0}]]],"version":"2.0"},
    "boundarySegments": [
      "299019",
      "299441"
    ],
    "id" : "313137",
    "levelId" : "1",
    "maxPt" : {
      "x" : -4938.068482562385,
      "y" : -3201.59397858169,
      "z" : 0.0
    },
    "minPt" : {
      "x" : -4938.068482562385,
      "y" : -3201.59397858169,
      "z" : 0.0
    },
    "name" : "dining room 4",
    "perimeter" : 28802.01,
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
    } ],
    "viewName" : "11"
  } ],
  "message" : null
}
```
