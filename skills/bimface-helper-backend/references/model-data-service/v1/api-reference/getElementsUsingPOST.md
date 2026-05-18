# 批量获取属性
```
POST https://api.bimface.com/data/v2/files/{fileId}/elements
```
### 说明
在请求体里可以传入构件ID列表来批量获取属性，单次请求的构件数量上限为1000。若需根据修改后的属性值查询，需要设置请求参数includeOverrides的值为true。

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
|includeOverrides|是否查询修改的属性|boolean|

##### Body<i class="require">*</i>
|名称|说明|类型|
|---|---|---|
|filter|过滤条件|< GroupAndKeysPair >array|
|  keys|样例："floor"|< string >array|
|  group|属性分组类型|string|
|elementIds|构件ID列表|< string >array|

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
|boundingBox|包围盒坐标|BoundingBox|
|min|最小坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|max|最大坐标点|Coordinate|
|x||double|
|y||double|
|z||double|
|name|名称|string|
|guid|全局唯一标识符|string|
|familyGuid|族标识符|string|
|properties|属性列表|< PropertyGroup >array|
|items|属性数据|< PropertyItem >array|
|extension|拓展信息|object|
|unit|单位|string|
|code|状态代码|string|
|orderNumber|示例: 0|int32|
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
https://api.bimface.com/data/v2/files/1938888813662976/elements
```


##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```


##### 请求 body
```json
{
  "elementIds" : [ "264531", "264668" ],
  "filter" : [ {
    "group" : "default"
  }, {
    "group" : "尺寸标注",
    "keys":["体积"]
  }, {
    "group" : "基本属性",
    "keys" : [ "floor"]
  } ]
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
            "boundingBox": {
                "max": {
                    "x": -1226.217,
                    "y": -7789.284,
                    "z": 10455.3
                },
                "min": {
                    "x": -1516.217,
                    "y": -8789.284,
                    "z": -450.0
                }
            },
            "elementId": "264531",
            "familyGuid": "",
            "guid": "88cd7355-b870-4d45-8fd7-51937085c0c6-00040953",
            "name": "常规EWALL - 290mm-stone 2",
            "properties": [
                {
                    "group": "基本属性",
                    "items": [
                        {
                            "key": "floor",
                            "value": "地坪"
                        }
                    ]
                },
                {
                    "group": "尺寸标注",
                    "items": [
                        {
                            "key": "体积",
                            "unit": "m³",
                            "value": "2.703969",
                            "valueType": 2
                        }
                    ]
                }
            ]
        }
    ]
}
```
