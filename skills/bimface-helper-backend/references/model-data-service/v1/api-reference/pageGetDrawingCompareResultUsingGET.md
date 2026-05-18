# 分页获取图纸对比结果
```
GET https://api.bimface.com/data/v2/comparisons/{compareId}/drawingdiff
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
|compareId *|图纸对比ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|layer|图层名称|string|
|page|页码|integer (int32)|
|pageSize|每页记录数|integer (int32)|
|sheetID|图纸ID|string|
|sheetName|图纸名称|string|
|type|图纸类型|string|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«Pagination«DrawingCompareDiff»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回示例|Pagination«DrawingCompareDiff»|
|total|条目总数|int32|
|data|对比结果数据|< DrawingCompareDiff >array|
|sheetName|图纸名称|string|
|diffType|差异类型|string|
|sheetID|图纸ID|string|
|id|图元ID|string|
|type|类型|string|
|layer|图层名称|string|
|page|页码|int32|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/comparisons/2077707858585731/drawingdiff
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
    "data" : [ {
      "diffType" : "CHANGE",
      "id" : "75331f2e3cc04f629b5387fc3743bfe4",
      "layer" : "layer",
      "sheetID" : "1",
      "sheetName" : "图纸1",
      "type" : "type"
    } ],
    "page" : 2,
    "total" : 10
  },
  "message" : null
}
```
