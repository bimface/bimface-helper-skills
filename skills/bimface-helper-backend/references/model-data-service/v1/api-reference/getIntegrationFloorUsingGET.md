# 计算指定构件列表的包围盒
```
GET https://api.bimface.com/data/integrations/{integrateId}/elements/boundingboxes
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

##### Body
|名称|说明|类型|
|---|---|---|
|fileIdWithEleIdList *|构件ID列表，由','分隔。每个构件ID由fileID和elementID组成|< string > array|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«ElementIdWithBoundingBox»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< ElementIdWithBoundingBox >array|
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
|message||string|

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/integrations/1738888866720224/elements/boundingboxes
```

##### 请求 header
```json
"Authorization: Bearer cn-e9725999-0b36-4c0e-bdca-38ea88888888"
```

##### 请求 body
```json
[
  "1938888813662976.218218",
  "1938888813662976.655533"
]
```

### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "data" : [ {
    "boundingBox" : {
      "max" : {
        "x": 7061.931,
        "y": -3201.594,
        "z": 2700.0
      },
      "min": {
        "x": 6859.931,
        "y": -18403.59,
        "z": -1200.0
      }
    },
    "elementId": "1938888813662976.218218"
  },
  {
    "boundingBox": {
      "max": {
        "x": 13162.93,
        "y": -15201.59,
        "z": 2700.0
      },
      "min": {
        "x": 6960.931,
        "y": -15403.59,
        "z": 2150.0
      }
    },
    "elementId": "1938888813662976.655533"
  } 
  ],
  "message" : null
}
```
