# 获取模型构件对比差异
```
GET https://api.bimface.com/data/v2/comparisons/{compareId}/elementChange
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
|compareId *|模型对比ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|followingElementId *|后一文件的构件ID|string|
|followingFileId *|后一文件的ID|integer (int64)|
|previousElementId *|前一文件的构件ID|string|
|previousFileId *|前一文件的ID|integer (int64)|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«ModelCompareChange»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|

##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|ModelCompareChange|
|changeQuantities||< Changed«Quantity» >array|
|_A||Quantity|
|unit|单位|string|
|code|返回码|string|
|qty||int32|
|name|名称|string|
|desc|描述|string|
|_B||Quantity|
|unit|单位|string|
|code|返回码|string|
|qty||int32|
|name|名称|string|
|desc|描述|string|
|_A|来源文件、构件ID|string|
|changeAttributes|修改属性列表|< Changed«Attribute» >array|
|_A||Attribute|
|unit|单位|string|
|value|参数值|string|
|key|特性|string|
|_B||Attribute|
|unit|单位|string|
|value|参数值|string|
|key|特性|string|
|_B|变更文件、构件ID|string|
|deleteAttributes|删除属性列表|< Attribute >array|
|unit|单位|string|
|value|参数值|string|
|key|特性|string|
|newAttributes|新增属性列表|< Attribute >array|
|unit|单位|string|
|value|参数值|string|
|key|特性|string|
|newQuantities||< Quantity >array|
|unit|单位|string|
|code|返回码|string|
|qty||int32|
|name|名称|string|
|desc|描述|string|
|deleteQuantities||< Quantity >array|
|unit|单位|string|
|code|返回码|string|
|qty||int32|
|name|名称|string|
|desc|描述|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`

### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/comparisons/2077707858585728/elementChange?followingElementId=296524&followingFileId=1938888813662976&previousElementId=296524&previousFileId=1938888813662975
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
    "_A" : "1938263713662976.294655",
    "_B" : "1938263713662982.294655",
    "changeAttributes" : [ {
      "_A" : {
        "key" : "尺寸标注-体积",
        "unit" : null,
        "value" : "1.33"
      },
      "_B" : {
        "key" : "尺寸标注-体积",
        "unit" : null,
        "value" : "1.330376"
      }
    } ],
    "changeQuantities" : null,
    "deleteAttributes" : [ {
      "key" : "限制条件-房间边界",
      "unit" : null,
      "value" : "True"
    } ],
    "deleteQuantities" : null,
    "newAttributes" : [ {
      "key" : "约束-房间边界",
      "unit" : null,
      "value" : "True"
    } ],
    "newQuantities" : null
  },
  "message" : null
}
```
