# 查询指定构件属性的所有值
```
GET https://api.bimface.com/data/v2/integrations/{integrateId}/propertyValues
```

### 说明
查询指定构件属性的所有值。

### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|bearer {accessToken}|string|
*为必填项

##### Path
|名称|说明|类型|
|---|---|---|
|integrateId *|集成ID|integer (int64)|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|includeOverrides|是否根据修改后的属性值查询，默认为false|boolean|
|properties *|需要查询的属性列表|< string > array(multi)|
*为必填项

### 响应

|HTTP代码|说明|类型|
|---|---|---|
|**200**|OK|GeneralResponse«List«PropertyValuesResp»»|
|**401**|Unauthorized|-|
|**403**|Forbidden|-|
|**404**|Not Found|-|


##### 200响应参数

|名称|说明|类型|
|---|---|---|
|code|状态代码|string|
|data|返回数据|< PropertyValuesResp >array|
|values|指定属性所有值的列表|< object >array|
|property|构件属性|string|
|message|提示消息|string|

### 生成

* `*/*`
* `application/json`


### HTTP请求示例

##### 请求 path
```
https://api.bimface.com/data/v2/integrations/1938888813662976/propertyValues?properties=floor
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
  "data": [
        {
            "property": "floor",
            "values": [
                "地坪",
                "ROOF",
                "F1",
                "F2",
                "F3"
            ]
        }
    ],
  "message" : null
}
```


