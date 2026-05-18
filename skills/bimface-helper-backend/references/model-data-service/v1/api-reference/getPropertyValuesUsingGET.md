# 查询指定模型构件属性的所有值
```
GET https://api.bimface.com/data/v2/query/propertyValues
```


### 说明
查询指定构件属性的所有值，同时支持单模型和集成模型，目标类型有两个可选值：file, integration。DSL 语法详见 [dsl-query.md](../../../dsl-query.md)。


### 参数
##### Header
|名称|说明|类型|
|---|---|---|
|Authorization *|bearer {accessToken}|string|
*为必填项

##### Query
|名称|说明|类型|
|---|---|---|
|includeOverrides|是否根据修改后的属性值查询，默认为false|boolean|
|properties *|需要查询的属性列表|< string > array(multi)|
|targetIds *|目标ID|< string > array(multi)|
|targetType *|目标类型 (默认值:"file")|string|
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
https://api.bimface.com/data/v2/query/propertyValues?properties=floor&targetIds=1938263713662976&targetType=file
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
  "message" : null,
  "data" : [ 
    {
      "property" : "floor",
      "values" : [  
        "F1",
        "F2",
        "F3",
        "ROOF",
        "地坪"
      ]
    } 
  ]
}
```


