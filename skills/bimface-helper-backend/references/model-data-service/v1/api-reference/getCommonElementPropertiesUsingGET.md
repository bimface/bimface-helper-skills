# 获取多个构件共同属性
```
GET https://api.bimface.com/data/v2/files/{fileId}/commonElementProperties
```


### 说明
若传入的elementId不止一个，则返回这些elementId共同的属性，共同的定义为：属性key与value都相等


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
|elementIds *|构件ID列表|< string > array(multi)|
|includeOverrides|是否查询修改的属性|boolean|

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
|orderNumber|样例：0|int32|
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
https://api.bimface.com/data/v2/files/1938888813662976/commonElementProperties?elementIds=string
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
  "message": null,
  "data" : {
    "boundingBox": {
            "max": {
                "x": -1226.22,
                "y": -7789.28,
                "z": 10455.30
            },
            "min": {
                "x": -1516.22,
                "y": -8789.28,
                "z": -450.0
            }
        },
    "elementId" : "313052",
    "familyGuid" : "000222",
    "guid" : "79d547c1-5dbf-4e6a-811d-951cf37b29da-0004c6dc",
    "name" : "norm - 150mm",
    "properties" : [ {
      "group" : "dimension",
      "items" : [ {
        "key" : "底部偏移",
        "orderNumber" : 0,
        "unit" : "mm",
        "value" : 0.0,
        "valueType" : 2
      } ]
    } ]
  }
}
```


