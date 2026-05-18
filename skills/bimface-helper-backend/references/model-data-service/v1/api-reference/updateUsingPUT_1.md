# 修改指定构件属性
```
PUT https://api.bimface.com/data/v2/integrations/{integrateId}/files/{fileIdHash}/elements/{elementId}/properties
```


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|elementId *|构件ID|string|
|fileIdHash *|文件ID|string|
|integrateId *|集成ID|integer (int64)|
*为必填项

##### Body
|名称|说明|类型|
|---|---|---|
|items||< PropertyItem >array|
|  extension||object|
|  unit|样例: "mm"|string|
|  code|样例: "perimeter"|string|
|  orderNumber||int32|
|  valueType|样例: 2|int32|
|  value|样例: 17200|object|
|  key|样例: "perimeter"|string|
|group|样例: "dimension"|string|

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«string»|
|**201**|Created|-|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|样例: "success"|string|
|data|样例: "data"|string|
|message|样例: ""|string|

### 消耗

* `application/json`


### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/integrations/1299498154893536/files/1299498154893536/elements/1299498/properties
```


##### 请求 header
```json
"Authorization: Bearer dc671840-bacc-4dc5-a134-97c1918d664b"
```


##### 请求 body
```json
[ {
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
```


### HTTP响应示例

##### 响应 200
```json
{
  "code" : "success",
  "data" : "data",
  "message" : ""
}
```


